# .Net Obfuscation

## Code Obfuscation

- .NET Obfuscator List

  https://github.com/NotPrab/.NET-Obfuscator

- Feature Introduction
  
  - [8 Ways To Protect And Obfuscate Your .Net Code Against Reverse-Engineering Using Crypto Obfuscator](https://www.ssware.com/articles/protect-and-obfuscate-your-dotnet-code-against-reverse-engineering-using-crypto-obfuscator.htm)

  - [Babel Obfuscation and Protection](https://www.babelfor.net/products/babel-obfuscator/)

- Feature Comparation

  | Component/Features | Obfuscar | ConfuserEx | Babel | Skater | .NET Reactor | Dotfuscator |
  | ------- | :-----: | :-----: | :-----: | :-----: | :-----: | :-----: |
  | Free | ✓ | ✓ | X | X | X | X |
  | OpenSource | ✓ | ✓ | X | X | X | X |
  | Symbol Renaming | ✓ |  ✓ |
  | Overloaded Renaming | ✓ |   |
  | Method Call Hiding | ✓ |
  | String Encryption | ✓ |
  | Control Flow Obfuscation | ✓ |
  | ILDASM Suppression | ✓ |
  | Anti-Decompiler | X |
  | Resource Encryption |  |
  | Assembly Encryption | ✓ |
  | Anti Debugging |  |
  
## Anti Debugging

Further research...

## Anti Dumping

Further research...

## Network Obfuscation

Further research...

## Code Deobfuscation

[de4dot](https://github.com/0xd4d/de4dot)

### Supported obfuscators/packers - 2020.10.12

* Agile.NET (aka CliSecure)
* Babel.NET
* CodeFort
* CodeVeil
* CodeWall
* CryptoObfuscator
* DeepSea Obfuscator
* Dotfuscator
* .NET Reactor
* Eazfuscator.NET
* Goliath.NET
* ILProtector
* MaxtoCode
* MPRESS
* Rummage
* Skater.NET
* SmartAssembly
* Spices.Net
* Xenocode

## Examples

### Obfuscar

- Install global tool

``` bash
dotnet tool install --global Obfuscar.GlobalTool
```

- Publish 

``` bash
# Console
dotnet publish -c Release -o publish

# WebApi
dotnet publish -c Release -o publish --self-contained true -r (win-x64|linux-x64|osx-x64)
```

- Prepare Config

``` xml
<!--config.xml-->
<?xml version='1.0'?>
<Obfuscator>
  <Var name="InPath" value="." />
  <Var name="OutPath" value=".\out" />
  <Var name="KeepPublicApi" value="false" />
  <Var name="HidePrivateApi" value="true" />
  <Var name="HideStrings" value="true" />
  <Var name="ReuseNames" value="true" />
  <Var name="RenameProperties" value="true" />
  <Var name="RenameEvents" value="true" />
  <Var name="RenameFields" value="true" />
  <Var name="UseUnicodeNames" value="true" />
  <Var name="OptimizeMethods" value="true" />
  <Var name="SuppressIldasm" value="true" />
  <Module file="$(InPath)\{your.dll}" >
    <!--Please uncomment below for WebApi-->
    <!--<SkipType name="ObfuscarWebApi.Startup" skipMethods="true" />-->
  </Module>
</Obfuscator>
``` 

- Obfuscate

``` bash
obfuscar.console config.xml
``` 
