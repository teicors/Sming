# Sming
Sming - Open Source framework for high efficiency WiFi SoC ESP8266 native development with C++ language.

[![ESP8266 C++ development framework](https://github.com/SmingHub/Sming/wiki/images/small/combine.png)](https://github.com/SmingHub/Sming/wiki/examples)

[![Gitter (chat)](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/SmingHub/Sming?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![Donate](http://img.shields.io/paypal/donate.png?color=yellow)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=WAQ8XDHCKU3PL&lc=US&item_name=Sming%20Framework%20development&item_number=sming&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted)
[![Download](https://img.shields.io/badge/download-~1.7M-orange.svg)](https://github.com/SmingHub/Sming/releases/latest)
[![Build](https://travis-ci.org/SmingHub/Sming.svg?branch=develop)](https://travis-ci.org/SmingHub/Sming)

## Summary
* Fast & user friendly development
* Work with GPIO in Arduino style
* High effective in performance and memory usage (this is native firmware!)
* Compatible with standard Arduino libraries - use any popular hardware in few lines of code
* rBoot OTA firmware updating
* Built-in file system: [spiffs](https://github.com/pellepl/spiffs)
* Built-in powerful network and wireless modules
* Built-in JSON library: [ArduinoJson](https://github.com/bblanchon/ArduinoJson)
* HTTP, AJAX, WebSockets support
* MQTT protocol based on [libemqtt](https://github.com/menudoproblema/libemqtt)
* Networking based on LWIP stack
* Simple and powerful hardware API wrappers
* Crash handlers for analyzing/handling system restarts due to fatal errors or WDT resets.
* SSL support based on [axTLS 2.1+](https://github.com/igrr/axtls-8266) with [Lwirax](https://github.com/attachix/lwirax/).
* Out of the box support for HTTP, MQTT and Websocket client connections over SSL. 
* Out of the box support for OTA over HTTPS.
* [SNI](https://tools.ietf.org/html/rfc6066#page-6) and [Maximum Fragment Length](https://tools.ietf.org/html/rfc6066#page-8) SSL support.
* Optional alternative PWM support based on [Stefan Bruens PWM](https://github.com/StefanBruens/ESP8266_new_pwm.git)
* Optional custom heap allocation based on [Umm Malloc](https://github.com/rhempel/umm_malloc.git)
* Based on Espressif NONOS SDK. Tested with versions 1.4, 1.5 and 2.0. 

## Compatibility

OS/SDK | Linux | Mac OS X | Windows | FreeBSD-current |
-------|-------|----------|---------|-----------------|
UDK (1.4, 1.5)    | n/a   | n/a      |   :sunny:      |     n/a         |
esp-alt-sdk (1.4, 1.5) | :sunny:  | :sunny:  | :sunny:  | :sunny:  | :sunny:  |
esp-open-sdk (1.4, 1.5, 2.0) | :sunny:  | :sunny: | n/a | n/a |

OS = Operating System
SDK = Software Development Kit
n/a = The selected SDK is not available on that OS

## Latest Release
- [Sming V3.0.1](https://github.com/SmingHub/Sming/releases/tag/3.0.1)

## Getting started
- [Windows](https://github.com/SmingHub/Sming/wiki/Windows-Quickstart)
- [Linux](https://github.com/SmingHub/Sming/wiki/Linux-Quickstart)
- [MacOS](https://github.com/SmingHub/Sming/wiki/MacOS-Quickstart)
- [Docker](https://github.com/SmingHub/Sming/wiki/Docker-Quickstart)


## Additional needed software 
- Spiffy  : Source included in Sming repository
- [ESPtool2](https://github.com/raburton/esptool2) esptool2 

## Optional features
- Custom LWIP:(default:ON) By default we are using custom compiled LWIP stack instead of the binary one provided from Espressif. This is increasing the free memory and decreasing the space on the flash. All espconn_* functions are turned off by default. If your application requires the use of some of the espconn_* functions then add the ENABLE_ESPCONN=1 directive. See `Makefile-user.mk` from the [Basic_SmartConfig](https://github.com/SmingHub/Sming/blob/develop/samples/Basic_SmartConfig/Makefile-user.mk#L41) application for examples. If you would like to use the binary LWIP then you should turn off the custom LWIP compilation by providing ENABLE_CUSTOM_LWIP=0.
- SSL:(default:off) The SSL support is not built-in by default to conserve resources. If you want to enable it then take a look at the [Readme](https://github.com/SmingHub/Sming/blob/develop/samples/Basic_Ssl/README.md) in the Basic_Ssl samples.
- Custom PWM:(default:off) If you want to use the [open PWM implementation](https://github.com/StefanBruens/ESP8266_new_pwm) then compile your application with ENABLE_CUSTOM_PWM=1. There is no need to recompile the Sming library.
- Custom serial baud rate: (default:off) The default serial baud rate is 115200. If you want to change it to a higher baud rate you can recompile Sming and your application changing the COM_SPEED_SERIAL directive. For example COM_SPEED_SERIAL=921600
- Custom Heap Allocation:(default:off) If your application is experiencing heap fragmentation then you can try the Umm Malloc heap allocation. To enable it compile Sming with ENABLE_CUSTOM_HEAP=1. In order to use it in your sample/application make sure to compile the sample with ENABLE_CUSTOM_HEAP=1. Avoid enabling your custom heap allocation AND -mforce-l32 compiler flag.
- Debug information log level and format: There are four debug levels: debug=3, info=2, warn=1, error=0. Using DEBUG_VERBOSE_LEVEL you can set the desired level (0-3). For example  DEBUG_VERBOSE_LEVEL=2 will show only info messages and above. Another make directive is DEBUG_PRINT_FILENAME_AND_LINE=1 which enables printing the filename and line number of every degub line. This will require extra space on flash. Note: You can compile the Sming library with a set of debug directives and your project with another settings, this way you can control debugging sepparately for sming and your application code.

You can find more information about compilation and flashing process by reading esp8266.com forum discussion thread.

## Examples
More information at **[Wiki Examples](https://github.com/SmingHub/Sming/wiki/examples)** page.

### Simple GPIO input/output
```c++
#define LED_PIN 2 // GPIO2
...
pinMode(LED_PIN, OUTPUT);
digitalWrite(LED_PIN, HIGH);
```

### Connect to WiFi and start Serial communication
```c++
Serial.begin(9600);
Serial.println("Hello Sming! Let's do smart things.");

WifiStation.enable(true);
WifiStation.config("LOCAL-NETWORK", "123456789087"); // Put you SSID and Password here
```

### Read DHT22 sensor
```c++
#include <Libraries/DHT/DHT.h> // This is just popular Arduino library!

#define WORK_PIN 0 // GPIO0
DHT dht(WORK_PIN, DHT22);

void init()
{
  dht.begin();

  float h = dht.readHumidity();
  float t = dht.readTemperature();
}
```

### HTTP client
```c++
HttpClient thingSpeak;
...
thingSpeak.downloadString("http://api.thingspeak.com/update?key=XXXXXXX&field1=" + String(sensorValue), onDataSent);

void onDataSent(HttpClient& client, bool successful)
{
  if (successful)
    Serial.println("Successful!");
  else
    Serial.println("Failed");
}
```

### OTA application update based on rBoot
```c++
void OtaUpdate() {
	
	uint8 slot;
	rboot_config bootconf;
	
	Serial.println("Updating...");
	
	// need a clean object, otherwise if run before and failed will not run again
	if (otaUpdater) delete otaUpdater;
	otaUpdater = new rBootHttpUpdate();
	
	// select rom slot to flash
	bootconf = rboot_get_config();
	slot = bootconf.current_rom;
	if (slot == 0) slot = 1; else slot = 0;

	// flash rom to position indicated in the rBoot config rom table
	otaUpdater->addItem(bootconf.roms[slot], ROM_0_URL);

	// and/or set a callback (called on failure or success without switching requested)
	otaUpdater->setCallback(OtaUpdate_CallBack);

	// start update
	otaUpdater->start();
}
```

### Embedded HTTP WebServer
```c++
server.listen(80);
server.addPath("/", onIndex);
server.addPath("/hello", onHello);
server.setDefaultHandler(onFile);

Serial.println("=== WEB SERVER STARTED ===");
Serial.println(WifiStation.getIP());

...

void onIndex(HttpRequest &request, HttpResponse &response)
{
  TemplateFileStream *tmpl = new TemplateFileStream("index.html");
  auto &vars = tmpl->variables();
  vars["counter"] = String(counter);
  vars["IP"] = WifiStation.getIP().toString();
  vars["MAC"] = WifiStation.getMAC();
  response.sendTemplate(tmpl);
}

void onFile(HttpRequest &request, HttpResponse &response)
{
  String file = request.getPath();
  if (file[0] == '/')
    file = file.substring(1);
    
  response.setCache(86400, true);
  response.sendFile(file);
}
```

### Documentation
A complete documentation can be created by running the command below. This requires `doxygen` to be installed on your system.

```
cd $SMING_HOME
make docs
```

The newly generated documentation will be located under Sming/docs/api
