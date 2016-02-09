# node-xcode-opifex

Modified for the OPengine

Primary change is Xcode 8 project support. (Not all functionality has been added yet, only that which the OPengine required)

> parser/toolkit for xcodeproj project files

Allows you to edit xcodeproject files and write them back out.

## Example

    var xcode = require('node-xcode-opifex'),
        fs = require('fs'),
        projectSource = 'OPengine.xcodeproj/project.pbxproj',
        projectPath = 'OPengine.xcodeproj/project.pbxproj',
        myProj = xcode.project(projectSource);

    // parsing is async, in a different process
    myProj.parse(function (err) {

    	// If it already exists, nothing happens
    	myProj.createPbxGroup('OPengine');

    	//myProj.addSourceFileToGroup('foo.m', 'OPengine');

    	myProj.addFrameworkFile('OPengine', 'libCore.a');
    	myProj.addFrameworkFile('OPengine', 'libData.a');
    	myProj.addFrameworkFile('OPengine', 'libMath.a');
    	myProj.addFrameworkFile('OPengine', 'libPerformance.a');
    	myProj.addFrameworkFile('OPengine', 'libHuman.a');
    	myProj.addFrameworkFile('OPengine', 'libCommunication.a');
    	myProj.addFrameworkFile('OPengine', 'libScripting.a');
    	myProj.addFrameworkFile('OPengine', 'libPipeline.a');
    	myProj.addFrameworkFile('OPengine', 'libApplication.a');

    	// Clears the defines for all of the Configs in 'teamopifex.OPengine'
    	myProj.clearConfigDefine('teamopifex.OPengine');
    	// Clears the defines for only the Debug config
    	myProj.clearConfigDefine('teamopifex.OPengine', 'Debug');

    	myProj.addConfigDefine('teamopifex.OPengine', 'OPIFEX_IOS');
    	myProj.addConfigDefine('teamopifex.OPengine', '_DEBUG', 'Debug');
    	myProj.addConfigDefine('teamopifex.OPengine', 'OPIFEX_RELEASE', 'Release');

    	myProj.clearConfigLibraryPath('teamopifex.OPengine');
    	myProj.addConfigLibraryPath('teamopifex.OPengine', '/Users/garretthoofman/.opengine/build/opengine_08c61120-44e7-4e5c-996d-88f7ec0875a6_build/Binaries/ios/debug', 'Debug');
    	myProj.addConfigLibraryPath('teamopifex.OPengine', '/Users/garretthoofman/.opengine/build/opengine_08c61120-44e7-4e5c-996d-88f7ec0875a6_build/Binaries/ios/release', 'Release');

        fs.writeFileSync(projectPath, myProj.writeSync());
    });

## Working on the parser

If there's a problem parsing, you will want to edit the grammar under
`lib/parser/pbxproj.pegjs`. You can test it online with the PEGjs online thingy
at http://pegjs.majda.cz/online - I have had some mixed results though.

Tests under the `test/parser` directory will compile the parser from the
grammar. Other tests will use the prebuilt parser (`lib/parser/pbxproj.js`).

To rebuild the parser js file after editing the grammar, run:

    ./node_modules/.bin/pegjs lib/parser/pbxproj.pegjs

(easier if `./node_modules/.bin` is in your path)

## License

MIT
