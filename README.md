# MUIBuild
This is a targets file for MSBuild that makes separation of language resources into .MUI files
easy and clean.

## How to use
1. Clone this repository into your project (as a submodule if using Git).
2. In your .vcxproj file, add the following lines before the `ItemGroup` element containing
   the project's `ResourceCompile` directives.
   ```xml
   <Import Project="$(SolutionDir)MUIBuild\MUIBuild.targets" />
   <PropertyGroup Label="MUI">
     <_PrimaryLanguage>en-US</_PrimaryLanguage>
     <_PrimaryLanguageID>0x0409</_PrimaryLanguageID>
     <_RCConfig>MUIExample.rcconfig</_RCConfig>
   </PropertyGroup>
   <Target Name="MUI" AfterTargets="Link">
     
   </Target>
   ```
   Adjust the project path as necessary to match your project structure.
3. For each language you wish to compile for your project, create a RC file with
   the following name format: `<ProjectName>.<Lang>.rc`. For example, `MUIExample.en-US.rc`,
   `MUIExample.ja-JP.rc`. Make sure they are excluded from the build, and also make sure they
   have the proper `LANGUAGE` directives.
4. Include the RC file for your primary language in your main RC file. For example:
   ```c
   #include "MUIExample.en-US.rc"
   ```
5. For each language that is not your primary language, add a `_BuildMUI` directive in the
   `Target` named `MUI` you pasted into your project file earlier. For example:
   ```xml
   <Target Name="MUI" AfterTargets="Link">
     <_BuildMUI Language="ja-JP" />
   </Target>
   ```