<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# Using the Latest WorldWind Features

## Accessing, building, and understanding the development version of WebWorldWind

The WebWorldWind project uses GitHub as a web-based hosting service for version control using git. The library uses RequireJS for loading javascript modules and npm is used as a build tool run tasks like: consolidating and minifying the library, generating documentation, and deploying artifacts.

To use the latest WebWorldWind library, you must be familiar with the software/services/concepts above. More information about each of the components can be found below:
 
 - [git and GitHub](https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners)
 - [RequireJS](http://requirejs.org/) - not necessary to fully understand in order to use the development version but does provide background on the file header configuration and how multiple files contribute to the library
 - [npm](https://docs.npmjs.com/getting-started/what-is-npm)
 
To build WebWorldWind from the latest source hosted on GitHub, follow these steps:

1. Install git and npm on your computer (varies by operating system)

2. Clone the WebWorldWind project which will copy the source code to your computer:
    
    ```
    git clone https://github.com/NASAWorldWind/WebWorldWind.git
    ```
    
3. Navigate into the newly created directory `WebWorldWind`:

    ```
    cd WebWorldWind
    ```
    
4. Invoke npm to install the required dependencies for building the different output components of the library:

    ```
    npm install
    ```

5. Invoke the npm build command for WebWorldWind to consolidate and minify library:

    ```
    npm run build
    ```
    
6. npm will now consolidate the files, minifying if necessary, and generate documentation. The outputs from this process are located in the `build` directory. Let's summarize the contents of the `build` directory:

    - `WebWorldWind-Distribution-0.9.0.zip`: A zipped file containing: License, Readme, documentation, examples, images, and the consolidated and minified libraries (worldwind.js and worldiwind.min.js).
    - `dist`: the unzipped contents of the WebWorldWind-Distribution zip file
    - `test-report`: test output reports
    
    To build your application you will need a minimum of one of the provided libraries (worldwind.js or worldwind.min.js). It is also recommended to include the images directory if you plan on displaying the `CompassLayer`, `ViewControlsLayer`, or `StarFieldLayer`. The images directory provides the images and data for those layers. The images directory should be placed at the root of the file server.
    
7. To retrieve the latest updates of WebWorldWind simply:

    ```
    git pull
    ```

    and then start back at step 5 to update the build products.
    
## Future Development Library Distribution Plan

The WebWorldWind development team has identified several alternatives for providing the development version of WebWorldWind. In the future, a precompiled version will always be available or integrated into npm's distribution mechanism.

# Next Steps
    
* [Home](../../)
* [Lesson 4.3: WorldWind Website](https://worldwind.arc.nasa.gov/)
