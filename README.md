# Example using QtAutoUpdate w/ CMake

## How to: 
- Using Qt 5.6.2 / QtAutoUpdater 1.1.0
- Download .lib files here: https://github.com/Skycoder42/QtAutoUpdater/releases/tag/1.1.0, find `QtAutoUpdater.lib` &  `QtAutoUpdaterController.lib` (in my case `QtAutoUpdater_precompiled\QtAutoUpdater\msvc64\release`) and copy them into the root of this directory.
- If building for another OS use libraries created for their particular runtime and update `target_linked_libraries` in `CMakeLists.txt`, being sure to copy the correct files for debug/release

## Example build MSVC64, release
```
mkdir build && cd build
cmake -G "Visual Studio 14 2015 Win64" ..
msbuild qtautoupdater-cmake.sln /p:Configuration=release
```

## main.cpp (the code invoking the library)
some API changes have been made to the "example" code I used in `main.cpp`, as such the example is modified from what is shown on (original github)[https://github.com/Skycoder42/QtAutoUpdater] README:

```
#include <QtCore/QCoreApplication>
#include <QtCore/QDebug>
#include <QtCore/QStandardPaths>
#include <updater.h>

int main(int argc, char *argv[])
{
	QCoreApplication a(argc, argv);

	QString homePath = QStandardPaths::writableLocation(QStandardPaths::HomeLocation);
	QtAutoUpdater::Updater *updater = new QtAutoUpdater::Updater(homePath + "/QtAutoUpdaterTestInstaller/maintenancetool",
																 nullptr);
	updater->runUpdaterOnExit();

	QObject::connect(updater, &QtAutoUpdater::Updater::checkUpdatesDone, [updater](bool a, bool b){
	
		qDebug() << "Has updates:" << a
				 << "\nHas errors:" << b
				 // change errorLog() to getErrorLog()
				 << "\nError string:" << updater->getErrorLog();
		qDebug() << updater->updateInfo();
		qApp->quit();
	});

	updater->checkForUpdates();
	return a.exec();
}
```

## To do:
- Would like to create a `FindQtQutoUpdater.txt` (for CMake to find and install libraries)
- Create CMakeText.txt to build and install various QtAutoUpdate libraries for different version of Qt