# Tailwind Css Demo

## Configuration

- Create a new Blazor Server project ```TailwindCssDemo```
- Open Folder in file Explorer
- Open Terminal

- Check npm installed or not
   
   ``` sh
   npm -v
   ```
   
2. Install npm (already installed skip this)
 
   ``` sh
   npm install
   ```
3. Create ```package.json``` file

   ``` sh
   npm init
   ```
   
4. Install packages ```tailwindcss```, ```cross-env```, ```postcss```, ```postcss-cli```, ```autoprefixer```, ```cssnano``` packages
   
   ``` sh 
   npm i -D tailwindcss cross-env postcss postcss-cli autoprefixer cssnano
   ```
   
6. Open ```package.json``` file and replace ```script``` section
   
   ``` json
    "scripts": {
     "buildcss:debug": "cross-env TAILWIND_MODE=build postcss ./Styles/tailwind.css -o ./wwwroot/css/tailwind.debug.css",
     "buildcss:release": "cross-env NODE_ENV=production postcss ./Styles/tailwind.css -o ./wwwroot/css/tailwind.release.css"
   },
   ```

   ```package.json```
   ``` json
   {
     "name": "tailwindcssdemo",
     "version": "1.0.0",
     "description": "Tailwind Css demo for Blazor Server",
     "main": "index.js",
     "scripts": {
       "buildcss:debug": "cross-env TAILWIND_MODE=build postcss ./Styles/tailwind.css -o ./wwwroot/css/tailwind.debug.css",
       "buildcss:release": "cross-env NODE_ENV=production postcss ./Styles/tailwind.css -o ./wwwroot/css/tailwind.release.css"
     },
     "author": "Manojbabu",
     "license": "ISC",
     "devDependencies": {
       "autoprefixer": "^10.4.16",
       "cross-env": "^7.0.3",
       "cssnano": "^6.0.1",
       "postcss": "^8.4.31",
       "postcss-cli": "^10.1.0",
       "tailwindcss": "^3.3.5"
      }
   }
   ```
   
8. Create ```tailwind.config.js``` file in project folder

   ```tailwind.config.js```
   ``` js
   module.exports = {
    content: ["./**/*.{razor,cshtml,js}"],
    theme: {
        extend: {},
    },
    plugins: [],
   }
   ```
   
9. Create a folder ```Styles``` in project folder
10. Create ```tailwind.css``` file in ```Styles``` folder
   
     ```tailwind.css```
     ``` css
     @tailwind base;
     @tailwind components;
     @tailwind utilities;
     ```
   
11. Create ```postcss.config.js``` file in project folder

     ```postcss.config.js```
     ``` js
     module.exports = ({ env }) => ({
      plugins: {
          tailwindcss: { },
          autoprefixer: { },
          cssnano: env === "production" ? { preset: "default" } : false
      },
     })
     ```
   
11. Check script is working or not

    ``` sh
    npm run buildcss:debug
    ```
    If script is run successful, it will create ```tailwind.debug.css``` file in your ```wwwwroot\css``` folder

12. Open your project file
    Add in ```<PropertyGroup>```
    
    ```xml
    <NpmLastInstall>node_modules/./last-install</NpmLastInstall>
    ```

    Add after ```<PropertyGroup>```

    ``` xml
    <Target Name="CheckNPMInstalled" BeforeTargets="InstallNPM">
	    <Exec Command="npm -v" ContinueOnError="true">
		  <Output TaskParameter="ExitCode" PropertyName="ErrorCode" />
	    </Exec>

	    <Error Condition="'$(ErrorCode)' != '0'" Text="You must install npm to run this task" />
    </Target>

    <Target Name="InstallNPM" BeforeTargets="BuildTailwindCSS">
    	<Exec Command="npm install" />
    	<Touch Files="$(NpmLastInstall)" AlwaysCreate="true" />
    </Target>

    <Target Name="BuildTailwindCSS" BeforeTargets="Compile">
    	<Exec Command="npm run buildcss:debug" Condition="'$(Configuration)' == 'Debug'" />
    	<Exec Command="npm run buildcss:release" Condition="'$(Configuration)' == 'Release'" />
    </Target>
    ```

    ```TailwindCssDemo.csproj```
    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

    	<PropertyGroup>
    		<TargetFramework>net7.0</TargetFramework>
    		<Nullable>enable</Nullable>
    		<ImplicitUsings>enable</ImplicitUsings>
    		<NpmLastInstall>node_modules/./last-install</NpmLastInstall>
    	</PropertyGroup>
    
    	<Target Name="CheckNPMInstalled" BeforeTargets="InstallNPM">
    		<Exec Command="npm -v" ContinueOnError="true">
    			<Output TaskParameter="ExitCode" PropertyName="ErrorCode" />
    		</Exec>
    
    		<Error Condition="'$(ErrorCode)' != '0'" Text="You must install npm to run this task" />
    	</Target>
    
    	<Target Name="InstallNPM" BeforeTargets="BuildTailwindCSS">
    		<Exec Command="npm install" />
    		<Touch Files="$(NpmLastInstall)" AlwaysCreate="true" />
    	</Target>
    
    	<Target Name="BuildTailwindCSS" BeforeTargets="Compile">
    		<Exec Command="npm run buildcss:debug" Condition="'$(Configuration)' == 'Debug'" />
    		<Exec Command="npm run buildcss:release" Condition="'$(Configuration)' == 'Release'" />
    	</Target>
	
    </Project>
    ```

13. Add link ```<link href="css/tailwind.debug.css" rel="stylesheet" /> ``` in ```_Host.cshtml```

    ```_Host.cshtml```
    ```cshtml
    @page "/"
    @using Microsoft.AspNetCore.Components.Web
    @namespace TailwindCssDemo.Pages
    @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
    
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="utf-8" />
        <base href="~/" />
        <link href="css/site.css" rel="stylesheet" />
        <link href="css/tailwind.debug.css" rel="stylesheet" /> 
        <component type="typeof(HeadOutlet)" render-mode="ServerPrerendered" />
    </head>
    <body>
        <component type="typeof(App)" render-mode="ServerPrerendered" />
    
        <div id="blazor-error-ui">
            <environment include="Staging,Production">
                An error has occurred. This application may no longer respond until reloaded.
            </environment>
            <environment include="Development">
                An unhandled exception has occurred. See browser dev tools for details.
            </environment>
            <a href="" class="reload">Reload</a>
            <a class="dismiss">🗙</a>
        </div>
    
        <script src="_framework/blazor.server.js"></script>
    </body>
    </html>
    ```

    
    
