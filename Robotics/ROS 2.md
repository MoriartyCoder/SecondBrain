# Basics

# Nodes
## Concept
A node represents a single task like reading a sensor or camera, controling a motor or planning the path.

## Basics
```
ros2 node list
```

```
ros2 node info <>
```

### Talker Basic
```
#include <chrono>       // Für Zeitfunktionen (z.B. 500ms)
#include <functional>   // Für std::bind(), um Callback-Funktionen zu binden
#include <memory>       // Für Smart Pointer (std::shared_ptr), wichtig in ROS 2
#include <string>       // Für String-Operationen

#include "rclcpp/rclcpp.hpp"    // Haupt-Header für die ROS 2 C++ Client Library (rclcpp)
#include "std_msgs/msg/string.hpp" // Definiert den Datentyp der zu sendenden Nachricht (String)

using namespace std::chrono_literals; // Erlaubt die Kurzschreibweise für Zeitdauern (z.B. 500ms)

/* Dieses Beispiel erstellt eine Unterklasse von rclcpp::Node und nutzt std::bind(), 
 * um eine Member-Funktion als Callback für den Timer zu registrieren. 
 * Dies ist die empfohlene Art, Nodes in C++ zu implementieren. */

class MinimalPublisher : public rclcpp::Node
{
  public:
    // **KONSTRUKTOR**
    // Ruft den Konstruktor der Basisklasse Node auf und gibt dem Node den Namen "minimal_publisher".
    MinimalPublisher() : Node("minimal_publisher"), count_(0)
    {
      // 1. Publisher erstellen: Publiziert Nachrichten vom Typ String an das Topic "topic". 
      //    '10' ist die QoS-Einstellung (Queue Depth / Warteschlangentiefe).
      publisher_ = this->create_publisher<std_msgs::msg::String>("topic", 10);
      
      // 2. Timer erstellen: Erzeugt einen Wand-Timer, der die Callback-Funktion alle 500 Millisekunden aufruft.
      timer_ = this->create_wall_timer(
      500ms, std::bind(&MinimalPublisher::timer_callback, this));
    }

  private:
    // **TIMER CALLBACK FUNKTION**
    // Diese Funktion wird alle 500ms aufgerufen und führt die Veröffentlichung aus.
    void timer_callback()
    {
      // Neues Nachrichtenobjekt vom Typ std_msgs::msg::String erstellen
      auto message = std_msgs::msg::String();
      
      // Nachrichteninhalt befüllen: "Hello, world!" und den aktuellen Zählerstand anhängen.
      message.data = "Hello, world! " + std::to_string(count_++);
      
      // Logging: Gibt die gesendete Nachricht im ROS 2 Log aus.
      RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
      
      // Nachricht über den erstellten Publisher veröffentlichen (senden)
      publisher_->publish(message);
    }
    
    // Member-Variablen (Speicher für Timer, Publisher und Zähler)
    rclcpp::TimerBase::SharedPtr timer_;                       // Smart Pointer für das Timer-Objekt
    rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_; // Smart Pointer für das Publisher-Objekt
    size_t count_;                                            // Zähler für die gesendeten Nachrichten
};

// **HAUPTPROGRAMM (MAIN-Funktion)**
int main(int argc, char * argv[]) {
  // 1. ROS 2 Initialisierung: Muss als Erstes in jedem ROS 2 Programm aufgerufen werden.
  rclcpp::init(argc, argv);
  
  // 2. Spin-Schleife starten: Erstellt eine Instanz des Publishers und startet die blockierende Hauptschleife.
  //    Die Spin-Schleife wartet auf Events (wie den Timer-Ablauf) und führt Callbacks aus.
  rclcpp::spin(std::make_shared<MinimalPublisher>());
  
  // 3. ROS 2 Aufräumen: Wird aufgerufen, wenn die Spin-Schleife beendet wird (z.B. durch Strg+C).
  rclcpp::shutdown();
  
  return 0;
}
```


# Topics
## Concept
Topics are the main communication mechanism. Nodes can publicate data on a topic and other nodes can subscribe. This is happening asyncron.

## Basics
```
ros2 topic list
```
# Services
## Concept
Services differ from topics. They are syncron and are working on a request/response-base. They can be used for configuration requests, triggers or status updates.

## Basics
```
ros2 service list
```
# Parameters
## Concept
Parameters are the single point of truth for configurations. Similar to enviromental variables.

## Basics
```
ros2 param list
```

```
ros2 param dump <node_name>
```

```
ros2 param get <node_name> <parameter_name>
```

```
ros2 param set <node_name> <parameter_name>
```

### Store and load parameters
Store: 
```
ros2 param dump <node_name> > <file_name>.yaml
```
Load:
```
ros2 param load <node_name> <file_name>.yaml
```

Load parameters on start of a node:
```
ros2 run <package_name> <executable_name> --ros-args --params-file <file_name>
```

# Something
## Create workspace
```
sudo apt update
sudo apt install python3-colcon-common-extensions

mkdir -p ~/<workspace_directory>/src
cd ~/<workspace_dircetory>
# Build workspace
colcon build --symlink-install
colcon test
# Source env (for each terminal!)
source install/setup.bash
```

## Create package
```
ros2 pkg create <package_name> --build-type ament_cmake --node-name <node_name>
```

## Build package
```
colcon build --packages-select <package_name>
source install/local_setup.bash
```
### Build package with debugging
```
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug --packages-select <package_name>
source install/local_setup.bash
```

## Debugging
### Basics
1. Packages
```
sudo apt update && apt install gdb gdbserver
```
2. Installing in VS Code the C/C++-Extension
3. Add to lauch.json
```
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "C++ Debugger",
			"request": "launch",
			"type": "cppdbg",
			"miDebuggerServerAddress": "localhost:3000",
			"cwd": "/",
			"program": "<build-path-executable>"
		}
	]
}
```
4. Run Node
```
ros2 run --prefix 'gdbserver localhost:3000' <package_name> <executable_name>
```
5. Take outputed path and replace **\<build-path-executable\>** in step 3.
6. Run the config 

### Note
The '**gdbserver localhost:3000**' can be added to the lauch file!
## Launch file
```
ros2 launch <package_name> <launch_file>
```
# Actions
## Concept
Actions are like the progessbar of an installation where you can see whats currently happening. While an action like reaching a destination or moving a part is active it sends out continous feedback. To do so a module has three subcomponents: goal, feedback and result. 

## Basics
```
ros2 action list
```