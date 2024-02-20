# ue5_python_api
Unreal Python API Documentation

### 1. `unreal.find_asset(name, type=Object, follow_redirectors=True) → Object`

- **Purpose**: This function is used to find an already loaded asset in UE5 by its name. You can specify the type of asset you're looking for (e.g., `Texture`, `Material`, `Blueprint`) to ensure the returned object is of the desired type. The `follow_redirectors` parameter helps in resolving any asset redirection that might have occurred (useful in asset management and refactoring scenarios).
- **Example**: Finding a specific material to apply to a mesh dynamically during runtime or in editor scripts.

    ```python
    material = unreal.find_asset("MyMaterial", unreal.Material)
    if material:
        print("Material found: ", material.get_name())
    else:
        print("Material not found.")
    ```

### 2. `unreal.load_asset(name, type=Object, follow_redirectors=True) → Object`

- **Purpose**: Similar to `find_asset` but loads the asset into memory if it's not already loaded. This is particularly useful for assets that are not initially loaded at runtime or for tools that need to work with assets dynamically without the user having to open them first.
- **Example**: Loading a texture dynamically to apply to an actor's material in a level.

    ```python
    texture = unreal.load_asset("/Game/Textures/MyTexture", unreal.Texture2D)
    if texture:
        print("Texture loaded: ", texture.get_name())
    ```

### 3. `unreal.new_object(type, outer=Transient, name=Default, base_type=Object) → Object`

- **Purpose**: Creates a new Unreal object of a specified class. You can define the object's outer (its parent/container), name, and ensure it inherits from a certain base type. This is fundamental for creating custom tools or runtime features that generate or manipulate content dynamically.
- **Example**: Creating a new custom actor in the world.

    ```python
    actor = unreal.new_object(unreal.Actor, unreal.get_editor_world(), "MyNewActor")
    if actor:
        print("New actor created: ", actor.get_name())
    ```

### 4. `unreal.generate_class(type) → None`

- **Purpose**: Generates an Unreal class from a given Python class. This is crucial for extending UE5 with custom logic that integrates deeply with the engine, allowing Python to define new types that behave as native UE5 classes.
- **Example**: Defining a new UObject subclass in Python and registering it with UE5.

    ```python
    @unreal.uclass()
    class MyCustomClass(unreal.Object):
        pass

    unreal.generate_class(MyCustomClass)
    ```

### 5. `unreal.log(str) → None`

- **Purpose**: Logs a message to the Unreal Engine log system. This is essential for debugging and providing feedback during the execution of Python scripts within UE5.
- **Example**: Logging a message when a particular operation is completed.

    ```python
    unreal.log("Operation completed successfully.")
    ```

### 6. `unreal.get_default_object(type) → Object`

- **Purpose**: Retrieves the Class Default Object (CDO) of a given Unreal class. This is useful for accessing default properties of a class without needing an instance of that class.
- **Example**: Getting the default walk speed of the character class.

    ```python
    character_cdo = unreal.get_default_object(unreal.Character)
    print("Default walk speed: ", character_cdo.get_editor_property("walk_speed"))
    ```

Given the extensive list of functions and the detailed explanation format requested, providing comprehensive explanations for each function would result in a very long response. However, I'll cover a few more key functions to demonstrate how to document them effectively. For the remaining functions, I recommend following the template and approach outlined in the initial set of explanations.

### 7. `unreal.get_editor_subsystem() → subsystem`

- **Purpose**: Retrieves an instance of an editor subsystem. Editor subsystems provide various functionalities within the UE5 editor, such as managing assets, levels, or the user interface. This function is crucial for scripts that aim to interact with or automate aspects of the editor's behavior.
- **Example**: Accessing the asset registry to find all assets of a certain type.

    ```python
    asset_registry = unreal.get_editor_subsystem(unreal.AssetRegistrySubsystem)
    all_materials = asset_registry.get_assets_by_class("Material")
    print(f"Found {len(all_materials)} materials.")
    ```

### 8. `unreal.is_editor() → Bool`

- **Purpose**: Checks whether the Unreal Editor is currently running. This is useful for scripts that should behave differently when running in the editor versus in a standalone game or during a packaged game session.
- **Example**: Conditional logic based on the environment.

    ```python
    if unreal.is_editor():
        print("Running in editor mode.")
    else:
        print("Running in game mode.")
    ```

### 9. `unreal.load_package(name) → Package`

- **Purpose**: Loads an Unreal package by name. Packages in UE5 are containers for assets and can include levels, blueprints, and other content. This function is particularly useful for tools that need to load or manipulate entire packages programmatically.
- **Example**: Loading a level package to modify its contents.

    ```python
    level_package = unreal.load_package("/Game/Levels/MyLevel")
    if level_package:
        print(f"Package {level_package.get_name()} loaded.")
    ```

### 10. `unreal.purge_object_references(obj, include_inners=True) → None`

- **Purpose**: Purges all references to a given Unreal object from any living Python objects. This is crucial for memory management and avoiding dangling references that can lead to crashes or unpredictable behavior, especially after deleting objects in the editor.
- **Example**: Cleaning up references after dynamically removing an actor.

    ```python
    actor_to_delete = unreal.find_asset("MyActor", unreal.Actor)
    unreal.EditorLevelLibrary.destroy_actor(actor_to_delete)
    unreal.purge_object_references(actor_to_delete)
    print("Actor and its references purged.")
    ```

### 11. `unreal.register_python_shutdown_callback(callable) → _DelegateHandle`

- **Purpose**: Registers a Python callable to be executed immediately before Python shuts down. This is useful for cleanup tasks or saving state before the Python environment is terminated.
- **Example**: Registering a shutdown callback.

    ```python
    def on_shutdown():
        print("Python environment is shutting down.")
    handle = unreal.register_python_shutdown_callback(on_shutdown)
    ```

Continuing with the detailed explanations for the Unreal Engine 5 Python API functions, let's dive into more of them:

### 12. `unreal.reload(str) → None`

- **Purpose**: Reloads a specified Unreal Engine Python module. This function is particularly useful during development, allowing developers to quickly test changes to their Python scripts without restarting the Unreal Editor.
- **Example**: Reloading a custom Python module named `my_module`.

    ```python
    unreal.reload("my_module")
    print("Module reloaded successfully.")
    ```

### 13. `unreal.get_type_from_class(class) → type`

- **Purpose**: Retrieves the best matching Python type for a given Unreal class. This function is crucial for scripts that need to dynamically work with Unreal objects and require type checking or casting based on the Unreal class.
- **Example**: Getting the Python type for a `StaticMesh` Unreal class.

    ```python
    python_type = unreal.get_type_from_class(unreal.StaticMesh)
    print(f"The Python type for StaticMesh is {python_type}.")
    ```

### 14. `unreal.get_type_from_enum(enum) → type`

- **Purpose**: Obtains the best matching Python type for a given Unreal enumeration (enum). This can be essential for scripts that need to interpret or set enum values from Python, ensuring type safety and correctness.
- **Example**: Getting the Python type for an enum `EVisibility`.

    ```python
    enum_type = unreal.get_type_from_enum(unreal.EVisibility)
    print(f"The Python type for EVisibility is {enum_type}.")
    ```

### 15. `unreal.log_error(str) → None`

- **Purpose**: Logs a given string as an error in the Unreal Engine's log system under the `LogPython` category. This is used for reporting errors encountered during the execution of Python scripts in UE5, helping in debugging and development.
- **Example**: Logging an error message when an operation fails.

    ```python
    unreal.log_error("Failed to load the specified asset.")
    ```

### 16. `unreal.log_warning(str) → None`

- **Purpose**: Logs a given string as a warning in the Unreal Engine's log system, similar to `log_error` but for less severe issues. This can be used to alert users of potential problems or misconfigurations in scripts without treating them as critical errors.
- **Example**: Warning about deprecated usage.

    ```python
    unreal.log_warning("This function is deprecated and will be removed in future versions.")
    ```

### 17. `unreal.log_flush() → None`

- **Purpose**: Flushes the log to disk, ensuring that all logged messages up to this point are saved. This is particularly useful in long-running scripts or processes where immediate log persistence is necessary.
- **Example**: Flushing logs after a critical operation.

    ```python
    unreal.log("Critical operation completed.")
    unreal.log_flush()
    ```

### 18. `unreal.ustruct()`

- **Purpose**: A decorator used to define UStruct types from Python. This allows for the creation of custom data structures that can be used within UE5, enabling complex data manipulation and storage directly from Python scripts.
- **Example**: Defining a new struct to hold game settings.

    ```python
    @unreal.ustruct()
    class GameSettings:
        health: unreal.int32
        ammo: unreal.int32
    
    unreal.generate_struct(GameSettings)
    print("Custom struct defined and generated.")
    ```

### 19. `unreal.load_class(outer, name, type=Object) → Class`

- **Purpose**: Loads an Unreal class with the given outer and name, optionally validating its base type. This function is useful for dynamically accessing or creating instances of Unreal classes from Python scripts.
- **Example**: Loading a custom actor class.

    ```python
    custom_actor_class = unreal.load_class(None, "CustomActor", unreal.Actor)
    if custom_actor_class:
        print(f"Custom Actor class loaded: {custom_actor_class.get_name()}")
    ```

### 20. `unreal.register_slate_pre_tick_callback(callable) → _DelegateHandle`

- **Purpose**: Registers a callable (function) to be executed as a pre-tick callback in Slate, Unreal's UI framework. This can be used for updating custom UI elements or executing logic before the UI is drawn each frame.
- **Example**: Registering a function to update UI elements.

    ```python
    def update_ui(delta_seconds):
        # Update UI logic here
        pass
    
    handle = unreal.register_slate_pre_tick_callback(update_ui)
    print("UI update callback registered.")
    ```

###21. unreal.unregister_slate_pre_tick_callback(handle) → None
Purpose: Unregisters a previously registered pre-tick callback in Slate, stopping the callback function from being executed before the UI is drawn each frame. This is useful for cleaning up or disabling UI updates when they are no longer needed.

Example: Unregistering a UI update function.

'''python

# Assuming 'handle' is the handle returned from a previous register call
unreal.unregister_slate_pre_tick_callback(handle)
print("UI update callback unregistered.")'''

###22. unreal.register_slate_post_tick_callback(callable) → _DelegateHandle
Purpose: Registers a callable (function) to be executed as a post-tick callback in Slate. This can be used for executing logic after the UI has been drawn each frame, such as cleanup or late updates to the UI.

Example: Registering a function to perform cleanup tasks after UI render.

'''python

def cleanup_ui(delta_seconds):
    # Cleanup logic here
    pass

handle = unreal.register_slate_post_tick_callback(cleanup_ui)
print("Post UI render cleanup callback registered.")'''

###23. unreal.unregister_slate_post_tick_callback(handle) → None
Purpose: Unregisters a previously registered post-tick callback in Slate. This function is necessary for removing callbacks that are no longer needed, preventing unnecessary executions and potential performance impacts.

Example: Unregistering a post-UI render cleanup function.

'''python

# Assuming 'handle' is the handle returned from a previous register call
unreal.unregister_slate_post_tick_callback(handle)
print("Post UI render cleanup callback unregistered.")'''

###24. unreal.register_python_shutdown_callback(callable) → _DelegateHandle
Purpose: Registers a Python callable to be executed immediately before Python shutdowns within Unreal Engine. This is particularly useful for scripts that need to perform cleanup or save operations before the Python environment is closed.

Example: Registering a shutdown cleanup function.

'''python

def on_python_shutdown():
    print("Performing cleanup before Python shuts down.")

handle = unreal.register_python_shutdown_callback(on_python_shutdown)
print("Shutdown callback registered.")'''

###25. unreal.unregister_python_shutdown_callback(handle) → None
Purpose: Unregisters a previously registered Python shutdown callback. This ensures that the callback function is no longer called during Python shutdown, which is useful for dynamically managing callbacks based on runtime conditions.

Example: Unregistering a shutdown cleanup function.

'''python

# Assuming 'handle' is the handle returned from a previous register call
unreal.unregister_python_shutdown_callback(handle)
print("Shutdown callback unregistered.")'''

###26. unreal.uproperty(type, meta=None, getter=None, setter=None)
Purpose: Defines a UProperty field from Python, allowing for the creation of custom properties that can be exposed to Unreal's editor and used within Blueprints or C++. This is critical for extending the functionality of custom classes with new data fields.

Example: Defining a custom property in a Python class.

'''python

class MyCustomActor(unreal.Actor):
    @unreal.uproperty(int, meta={"ToolTip": "An example integer property."})
    def my_int_property(self):
        return self._my_int_property

    @my_int_property.setter
    def my_int_property(self, value):
        self._my_int_property = value"
'''
###27. unreal.ufunction(meta=None, ret=None, params=None, override=None, static=None, pure=None, getter=None, setter=None)
Purpose: Decorator used to define UFunction fields from Python. UFunctions are functions that can be called from Blueprints or C++, and this decorator allows Python methods to be exposed in such a way. This is essential for integrating Python logic into the broader UE scripting ecosystem.

Example: Exposing a Python method as a UFunction.

'''python

class MyCustomActor(unreal.Actor):
    @unreal.ufunction(ret=int, params=[int, int], meta={"Category": "Math Operations"})
    def add_numbers(self, a, b):
        return a + b
'''

###28. unreal.get_engine_subsystem() → subsystem
Purpose: This function is used to retrieve a specific engine subsystem instance. Engine subsystems offer a variety of lower-level functionalities that are global to the engine, such as the gameplay, rendering, and audio systems. This is crucial for scripts that need to interact with or modify the engine's core functionalities at runtime.

Example: Accessing the audio system to change global audio settings.

'''python

audio_system = unreal.get_engine_subsystem(unreal.AudioSubsystem)
if audio_system:
    audio_system.set_master_volume(0.5)
    print("Master volume set to 50%.")
'''

###29. unreal.get_interpreter_executable_path() → str
Purpose: Retrieves the file path to the Python interpreter executable that the Unreal Engine's Python plugin was compiled against. This can be essential for scripts that need to spawn separate Python processes or for debugging purposes to ensure compatibility with the correct Python version.

Example: Printing the path of the Python interpreter.

'''python

interpreter_path = unreal.get_interpreter_executable_path()
print(f"Python interpreter path: {interpreter_path}")'''

###30. unreal.get_type_from_struct(struct) → type
Purpose: Obtains the best matching Python type for a given Unreal struct. This function is important for dynamic scripting where Python code needs to work with custom Unreal structures, ensuring that Python and UE4 types are correctly mapped.

Example: Getting the Python type for a custom struct named MyCustomStruct.

'''python

struct_type = unreal.get_type_from_struct(unreal.MyCustomStruct)
print(f"The Python type for MyCustomStruct is {struct_type}.")'''

###31. unreal.load_module(str) → None
Purpose: Loads a specified Unreal module and generates Python bindings for its reflected types. This is particularly useful for extending scripts with functionalities from different parts of the engine or custom modules not loaded by default.

Example: Loading a custom gameplay module named "MyGameplayModule".

'''python

unreal.load_module("MyGameplayModule")
print("MyGameplayModule loaded and Python bindings generated.")'''

###32. unreal.LOCTABLE(id, key) → Text
Purpose: Retrieves a localized text from the given string table ID and key. This is crucial for developing internationalized applications within Unreal, allowing scripts to programmatically access and use localized content.

Example: Fetching localized text for a UI element.

'''python

localized_text = unreal.LOCTABLE("MyStringTableID", "WelcomeMessage")
print(f"Localized welcome message: {localized_text}")'''

###33. unreal.NSLOCTEXT(ns, key, source) → Text
Purpose: Creates a localized text using the specified namespace, key, and source string. This function is used when defining new localized texts within a script, providing a way to support multiple languages in game content or UI dynamically.

Example: Creating a new localized text entry for a game notification.

'''python

notification_text = unreal.NSLOCTEXT("GameNotifications", "HealthLow", "Warning: Health Low!")
print(f"Localized notification text: {notification_text}")'''

###34. unreal.parent_external_window_to_slate(external_window, parent_search_method=SlateParentWindowSearchMethod.ACTIVE_WINDOW) → None
Purpose: Parents an OS-specific external window handle to a suitable Slate window, based on the search method provided. This is useful for integrating third-party applications or custom UI elements into the Unreal Editor's windowing system.

Example: Assuming you have an external window handle, parent it to the active Slate window.

'''python

# This is a hypothetical example, as obtaining a handle would depend on external APIs
external_window_handle = get_external_window_handle()
unreal.parent_external_window_to_slate(external_window_handle)
print("External window parented to an active Unreal Slate window.")'''

###35. unreal.purge_object_references(obj, include_inners=True) → None
Purpose: Purges all references to a given Unreal object from any living Python objects. This function is essential for managing memory and preventing leaks by ensuring that objects are properly garbage collected when no longer needed.

Example: Purging references to a deleted actor.

'''python

actor = unreal.find_asset("MyActor", unreal.Actor)
unreal.EditorLevelLibrary.destroy_actor(actor)
unreal.purge_object_references(actor)
print("References to the actor have been purged.")'''

###36. unreal.uclass()
Purpose: The unreal.uclass() decorator is used to define UClass types from Python, allowing developers to create new Unreal Engine classes directly from Python scripts. These classes can then be instantiated as objects within the Unreal Engine ecosystem, fully integrating with the engine's type system and enabling a wide range of functionalities from gameplay mechanics to editor extensions.

Example: Defining a new Unreal Engine class for a custom actor.

'''python

@unreal.uclass()
class MyCustomActor(unreal.Actor):
    """A simple custom actor defined in Python."""
'''

###37. unreal.uenum()
Purpose: This decorator is utilized to define UEnum types from Python, facilitating the creation of enumerated types (enums) that are recognized by Unreal Engine. Enums defined this way can be used to create readable, discrete sets of values for variables in UE scripts and blueprints, enhancing code clarity and maintainability.

Example: Creating an enum for character states.

'''python

@unreal.uenum()
class CharacterState:
    IDLE = 0
    RUNNING = 1
    JUMPING = 2
    ATTACKING = 3
'''
###38. unreal.ufunction(meta=None, ret=None, params=None, override=None, static=None, pure=None, getter=None, setter=None)
Purpose: The unreal.ufunction() decorator is designed to define UFunction fields from Python. UFunctions are functions that are callable within the UE scripting environment, including blueprints and C++. This decorator allows Python methods to be exposed to UE as functions, enabling complex logic to be implemented in Python and seamlessly integrated into the engine's workflow.

Example: Exposing a Python method as a callable function in Unreal Engine.

'''python

class MyActor(unreal.Actor):
    @unreal.ufunction(ret=int, params=[int, int], meta={"Category": "CustomMath"})
    def add(self, a, b):
        """Adds two numbers and returns the result."""
        return a + b
'''

###39. unreal.uproperty(type, meta=None, getter=None, setter=None)
Purpose: This function is used to define UProperty fields from Python, allowing the creation of properties that can be exposed to the Unreal Engine editor and used within blueprints or C++ code. It provides a way to add editable, serializable fields to custom classes, enriching the interaction between Python scripts and the engine.

Example: Adding a property to a custom actor class.

'''python

class MyCustomActor(unreal.Actor):
    @unreal.uproperty(int, meta={"Category": "Gameplay", "ToolTip": "Health points of the actor"})
    def health(self):
        return self._health

    @health.setter
    def health(self, value):
        self._health = value
'''

###40. unreal.ustruct()
Purpose: The unreal.ustruct() decorator is used to define UStruct types from Python, enabling the creation of structured data types that are recognized by Unreal Engine. Structs are useful for grouping related data together, and when defined in Python, they can be utilized both in scripting and within the UE editor for data management and manipulation.

Example: Defining a struct for game settings.

'''python

@unreal.ustruct()
class GameSettings:
    """Struct to hold various game settings."""
    resolution: unreal.FVector2D
    fullscreen: bool
    volume: float
'''

###41. unreal.uvalue(val, meta=None)
Purpose: This function is utilized to define constant values from Python, making them available within the Unreal Engine. It allows for the definition of named constants that can be used for comparison, configuration, and as parameters in functions and blueprints, ensuring consistency across the project.

Example: Defining a game version constant.

'''python

GAME_VERSION = unreal.uvalue("1.0.0")
'''

###42. unreal.new_object(type, outer=Transient, name=Default, base_type=Object) → Object
Purpose: Creates a new Unreal object of the specified class, optionally within a specified outer container and with a specific name. This is a foundational operation for scripts that dynamically generate content, manipulate scene hierarchy, or create utility objects at runtime or within the editor.

Example: Creating a new actor in the current level.

'''python

new_actor = unreal.new_object(unreal.Actor, unreal.get_editor_world(), name="MyNewActor")
print(f"New actor created: {new_actor.get_name()}")
'''

###43. unreal.reload(str) → None
Purpose: Reloads a specified Unreal Python module, refreshing its state and code. This is crucial during development, allowing for rapid iteration on Python scripts without needing to restart the Unreal Engine editor.

Example: Reloading a custom module after making changes.

'''python

module_name = "my_custom_module"
unreal.reload(module_name)
print(f"Module {module_name} reloaded.")
'''

###44. unreal.unregister_python_shutdown_callback(handle) → None, unreal.unregister_slate_post_tick_callback(handle) → None, unreal.unregister_slate_pre_tick_callback(handle) → None
Purpose: These functions unregister previously registered callbacks for Python shutdown, Slate post-tick, and pre-tick events, respectively. They are essential for managing the lifecycle of scripts and UI elements that interact with the Unreal Engine editor or runtime, ensuring that resources are properly cleaned up and that callbacks do not persist beyond their intended lifespan.

Example: Unregistering a shutdown callback.

'''python

# Assuming 'handle' is a previously obtained handle from registering a callback
unreal.unregister_python_shutdown_callback(handle)
print("Shutdown callback unregistered.")
'''

unreal._EnumEntry
Purpose: The unreal._EnumEntry class represents a single entry in an enumeration within Unreal Engine, encapsulating both the name and value of the enum entry. It is a fundamental part of the Unreal Engine Python API, providing a structured way to interact with enum values, facilitating readability and maintainability of scripts that deal with enum-based properties or logic.

Example: Accessing an enum entry's name and value.

'''python

# Example enum entry access
my_enum_entry = unreal._EnumEntry(enum="ECharacterState", name="IDLE", value=0)
print(f"Enum Name: {my_enum_entry.name}, Enum Value: {my_enum_entry.value}")
In this simplified example, unreal._EnumEntry is used to represent an enum entry for a hypothetical character state enum (ECharacterState). The name and value properties provide direct access to the enum entry's information, useful for debugging, logging, or conditional logic based on enum states.
'''

unreal._Logger
Purpose: The unreal._Logger class provides a Python interface for logging within the Unreal Engine environment, utilizing specified functions for logging and flushing log messages. It is designed to integrate Python's logging mechanism with Unreal's logging system, offering a streamlined way to output log messages, errors, or warnings from Python scripts directly to Unreal's log console. This class is especially useful for custom logging setups where developers need precise control over the logging and flushing process.

log_func: A function or callable that is used to log messages.
flush_func: A function or callable that is used to flush the log buffer, ensuring that all pending log messages are written out.
Methods:

flush(): Calls the flush_func provided during initialization to flush the log buffer.
write(log_text): Logs a single line of text using the log_func provided during initialization.
writelines(lines): Logs multiple lines of text, iterating over the lines iterable and logging each item.
Example Usage:

'''python

# Example setup for a custom logger in Unreal Engine Python scripts

# Define custom logging functions
def custom_log_func(message):
    unreal.log(f"Custom Log: {message}")

def custom_flush_func():
    unreal.log_flush()

# Create an instance of the _Logger with custom functions
custom_logger = unreal._Logger(log_func=custom_log_func, flush_func=custom_flush_func)

# Using the logger
custom_logger.write("This is a test log message.")
custom_logger.writelines(["First line of log", "Second line of log"])
custom_logger.flush()  # Ensures all messages are flushed to Unreal's log console

print("Custom logging operations completed.")'''

unreal._ObjectBase
Purpose: unreal._ObjectBase serves as the base class for all Unreal Engine objects exposed to Python. It provides a foundational interface for interacting with these objects, offering methods for reflection-based method calls, property access, class information retrieval, and more. This class is crucial for working with Unreal Engine's object system within Python, allowing scripts to manipulate game objects, access their properties, and call their methods dynamically.

Key Methods:

call_method(name, args=tuple(), kwargs=dict()) → object: Calls a method on the Unreal object using reflection, useful for invoking methods that aren't directly exposed to Python.
cast(object) → Object: Casts a given object to this Unreal object type, enabling type-safe operations on objects.
get_class() → Class: Retrieves the Unreal class of the instance, useful for reflection and runtime type checking.
get_default_object() → Object: Gets the class default object (CDO) of this type, providing access to default properties and settings.
get_editor_property(name) → object: Fetches the value of any property visible to the editor, facilitating easy access to object properties within scripts.
get_name() → str, get_full_name() → str, get_path_name() → str: Various methods for retrieving the name and path of the instance, important for identification and asset management.
get_outer() -> Object, get_outermost() → Package: Methods for navigating the object's hierarchy, either to its immediate outer object or the outermost package.
modify(bool) -> bool: Marks the instance as about to be modified, integrating with Unreal's undo/redo system.
set_editor_property(name, value, notify_mode=PropertyAccessChangeNotifyMode.DEFAULT) → None: Sets the value of any property visible to the editor, with support for change notifications.
Example Usage:

'''python

# Accessing and manipulating an actor's properties via _ObjectBase methods

# Assuming 'actor' is an Unreal Engine actor object
actor = unreal.EditorLevelLibrary.get_selected_level_actors()[0]

# Get and set properties using get_editor_property and set_editor_property
initial_location = actor.get_editor_property("location")
print(f"Initial Location: {initial_location}")

# Modify the actor's location
new_location = unreal.Vector(100, 200, 300)
actor.set_editor_property("location", new_location, unreal.PropertyAccessChangeNotifyMode.ALWAYS)
print("Actor location updated.")

# Dynamically calling a method on the actor
result = actor.call_method("GetActorLocation")
print(f"Actor Location via call_method: {result}")'''

unreal._WrapperBase
Purpose: The unreal._WrapperBase class serves as the foundational base class for all Unreal Engine types exposed to Python. This class underpins the Python integration with Unreal Engine, providing core functionalities that enable Python scripts to interact seamlessly with Unreal Engine's object model and type system. It's designed to encapsulate the essential behaviors and attributes that are common across all exposed Unreal Engine types, making it a critical component of Unreal's Python API.

Key Characteristics:

It acts as the superclass for a wide range of Unreal Engine objects accessible from Python, including actors, components, and other UObject-derived classes.
Provides basic initialization and common functionality that is extended by more specific subclasses to represent the diverse types within Unreal Engine.
Although it's rare for developers to interact directly with _WrapperBase in everyday scripting, understanding its role is crucial for advanced scripting, custom tool development, and extending the Python API with new functionalities.
Example Usage:

While you typically won't instantiate or directly interact with unreal._WrapperBase, understanding its place in the hierarchy is important for creating custom classes or extending existing ones within the Unreal Python API. Here's a conceptual example to illustrate how unreal._WrapperBase fits into the Python API's class hierarchy:

'''python

# Conceptual illustration of extending an Unreal Engine type in Python

class MyCustomActor(unreal.Actor):
    """
    This custom actor class extends unreal.Actor, which indirectly inherits
    from unreal._WrapperBase, inheriting its foundational behaviors.
    """
    
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        # Initialization code specific to MyCustomActor

# Usage of MyCustomActor would follow Unreal's Python API conventions,
# benefiting from the functionalities provided by _WrapperBase through inhe
'''

unreal.ActorIterator
Purpose: The unreal.ActorIterator class is designed for iterating over Unreal actor instances in a level or scene. This iterator type is essential for scripts that need to perform operations on multiple actors, such as searching, filtering, or batch processing based on specific criteria. By providing a Pythonic way to iterate over actors, it simplifies the task of managing scene contents programmatically within the Unreal Engine environment.

Key Characteristics:

Enables efficient traversal of all actor instances in the current level or specified levels.
Supports filtering actors by class, allowing developers to iterate over actors of a specific type.
Facilitates bulk operations on actors, such as attribute modification, component addition/removal, or event triggering.
Example Usage:

Here's a simple example illustrating how to use unreal.ActorIterator to iterate over all actors of a specific type in the current level, modifying a property for each:

'''python

# Example: Iterating over all StaticMeshActors in the current level and printing their names

for actor in unreal.ActorIterator(unreal.StaticMeshActor):
    print(actor.get_name())

    # Example modification: Hiding all StaticMeshActors
    actor.set_actor_hidden_in_game(True)

    # Committing changes if necessary (e.g., for undo/redo stack)
    actor.modify()
    actor.post_edit_change()
    
print("Processed all StaticMeshActors.")'''

unreal.Array
Purpose: The unreal.Array class is tailored for handling Unreal Engine array instances in Python, mirroring the functionality and behavior of TArray in C++. It provides a Pythonic interface to Unreal Engine's native arrays, allowing for the manipulation of array data with common Python operations. This class is instrumental for scripts that interact with Unreal properties or require array manipulation, offering methods to modify and query the array efficiently.

Key Methods:

__copy__() / copy(): Creates a shallow copy of the Unreal array.
append(value): Adds a value to the end of the array, similar to TArray::Add in C++.
cast(type, obj): Casts the given object to the specified Unreal array type.
count(value): Returns the count of occurrences of a value in the array.
extend(iterable): Extends the array with elements from the given iterable, akin to TArray::Append in C++.
index(value, start=0, stop=len): Finds the index of a value in the array, with optional start and stop parameters for the search range.
insert(index, value): Inserts a value at the specified index in the array.
pop(index=len - 1): Removes and returns the element at the given index, or the last element by default.
remove(value): Removes the first occurrence of a value in the array.
resize(len): Adjusts the size of the array to the specified length, potentially adding default elements or truncating.
reverse(): Reverses the array in place.
sort(key=None, reverse=False): Performs an in-place sort of the array, with optional key function and reverse order parameters.
Example Usage:

'''python

# Example: Creating an Unreal Array of integers and manipulating it

# Create an Unreal Array of integers
int_array = unreal.Array([1, 2, 3, 4, 5])

# Append a new element
int_array.append(6)

# Extend the array with more elements
int_array.extend([7, 8, 9])

# Remove an element
int_array.remove(3)

# Insert an element at a specific index
int_array.insert(0, 10)

# Pop the last element
last_element = int_array.pop()

# Sort the array in reverse order
int_array.sort(reverse=True)

# Print the final array state
print(f"Final array state: {int_array}")'''

unreal.AutomationScheduler
Purpose: The unreal.AutomationScheduler class is designed to manage the scheduling and execution of Python functions and generators within the Unreal Editor, allowing them to run across editor ticks. This facilitates the creation of non-blocking, latent operations that can yield execution back to the editor between steps. It's particularly useful for automating tasks that require waiting for editor responses or events, such as loading assets, setting up levels, or any operation that benefits from being spread out over time to keep the editor responsive.

Key Methods:

add_latent_command(task): Adds a function or generator to the scheduler. Functions will be executed once, while generators will have one iteration executed per tick. This method can also be used as a decorator to easily schedule functions for execution.

cleanup(): Forces the shutdown of the singleton scheduler instance, if it is running. This is useful for cleaning up pending tasks when they are no longer needed or before starting a new batch of tasks.

set_latent_command_timeout(seconds): Sets a timeout for Python Automation Test latent commands. This timeout specifies how long the scheduler should wait for a latent command to complete before considering it failed or stalled.

Example Usage:

Scheduling a simple function:

‘’’python

@unreal.AutomationScheduler.add_latent_command
def print_message():
    print("This message is printed between editor ticks.")’’’

Using a generator for tasks that require waiting between steps:

'''python

@unreal.AutomationScheduler.add_latent_command
def setup_level():
    print("Starting level setup...")
    yield unreal.EditorAssetLibrary.load_asset("/Game/Path/To/SomeAsset")
    print("Asset loaded. Continuing setup...")
    yield  # Yielding with no value simply waits for the next tick.
    print("Level setup complete.")'''

Cleanup before exiting or resetting:

'''python

# Force cleanup of the AutomationScheduler to ensure no tasks are left pending
unreal.AutomationScheduler.cleanup()'''
Setting a timeout for latent commands:

'''python

# Set a timeout of 10 seconds for latent commands
unreal.AutomationScheduler.set_latent_command_timeout(10)'''

unreal.ClassIterator
Purpose: The unreal.ClassIterator class is designed for iterating over Unreal Engine class types. This iterator facilitates the process of enumerating through all loaded Unreal classes within the editor or during runtime, providing a Pythonic way to access and interact with the Unreal Engine's reflection system. It's particularly useful for operations that require knowledge or manipulation of various Unreal Engine classes, such as dynamic instantiation, type inspection, or building custom editor tools that operate across a wide range of object types.
Example Usage:
'''python

# Example: Iterating over all subclasses of unreal.Actor

for cls in unreal.ClassIterator():
    # Ensure the class is a subclass of unreal.Actor (excluding unreal.Actor itself)
    if issubclass(cls, unreal.Actor) and cls != unreal.Actor:
        print(cls.get_name())

# This loop prints the names of all loaded classes that are subclasses of unreal.Actor,
# demonstrating how to dynamically access and work with Unreal Engine's class hierarchy.'''

unreal.DelegateBase
Purpose: The unreal.DelegateBase class serves as the foundation for all delegate instances exposed to Python by Unreal Engine. Delegates are a key part of Unreal Engine's event handling and callback system, allowing for the dynamic binding and execution of functions in response to various events or conditions within the game or application. This class provides the mechanisms to interact with, bind, and invoke these delegates from Python, bridging the functionality of Unreal Engine's C++ delegates with Python's callable objects.

Key Methods:

__copy__() / copy(): Creates a shallow copy of the delegate, allowing for the duplication of delegate bindings without affecting the original.
bind_callable(callable): Binds the delegate to a Python callable object, enabling the delegate to invoke Python functions or methods when executed.
bind_delegate(delegate): Binds the delegate to another delegate, effectively duplicating the binding of the original delegate.
bind_function(obj, name): Binds the delegate to a named function on a given Unreal object instance, allowing for the invocation of Unreal Engine functions.
cast(object): Casts the given object to this Unreal delegate type, useful for type conversion and ensuring proper delegate handling.
execute(...): Executes the delegate, triggering an error if the delegate is unbound and therefore has no function to call.
execute_if_bound(...): Safely executes the delegate if it is bound to a function, otherwise does nothing, avoiding errors from unbound delegate calls.
is_bound(): Checks if the delegate is currently bound to a function or callable, indicating whether it is ready to be executed.
unbind(): Removes any current binding from the delegate, making it unbound and preventing it from executing any function until it is bound again.
Example Usage:

'''python

# Example: Using unreal.DelegateBase to bind and execute a Python callable

# Define a Python function to be called by the delegate
def my_python_callable():
    print("Python callable executed by Unreal delegate.")

# Create an instance of an Unreal delegate (assume MyDelegateType exists and is derived from DelegateBase)
my_delegate = unreal.MyDelegateType()

# Bind the delegate to the Python callable
my_delegate.bind_callable(my_python_callable)

# Check if the delegate is bound
if my_delegate.is_bound():
    print("Delegate is bound, executing...")
    # Execute the delegate, which will call my_python_callable
    my_delegate.execute()
else:
    print("Delegate is not bound.")

# Unbinding the delegate
my_delegate.unbind()'''

unreal.EnumBase
Purpose: The unreal.EnumBase class represents the foundation for all enumeration (enum) instances exposed to Python by Unreal Engine. Enums in Unreal Engine are used to define a set of named integer constants, providing a readable and efficient way to represent various states, options, or categories within the engine and game code. The EnumBase class provides Python scripts with the ability to interact with these enums, offering functionality to cast objects to enum types and access the static enum definition.

Key Methods:

cast(object) → enum: Casts the given Python object to the specific Unreal enum type represented by this EnumBase class. This method is useful for ensuring that a Python object is treated as the correct enum type within scripts, especially when dealing with generic or dynamically typed data.
static_enum() → Enum: Retrieves the static enum object associated with this EnumBase type. This method is essential for accessing the underlying Unreal Engine enum definition, including its members and values, directly from Python. It allows scripts to reflect on the enum, such as listing its possible values or converting between names and values.
Example Usage:

'''python

# Example: Working with an Unreal Engine enum in Python

# Assuming there's an Unreal Engine enum named "EGameplayState" exposed to Python
# EGameplayState might have values like IDLE, RUNNING, PAUSED, etc.

# Access the static enum definition for EGameplayState
gameplay_state_enum = unreal.EGameplayState.static_enum()

# Iterate over all enum members and print their names and values
for name in gameplay_state_enum.names():
    value = gameplay_state_enum.get_value(name)
    print(f"Name: {name}, Value: {value}")

# Casting an object to the enum type (useful in dynamic or generic contexts)
# Assume 'some_value' is a variable that is expected to be an instance of EGameplayState
casted_value = unreal.EGameplayState.cast(some_value)
print(f"Casted Value: {casted_value}")

# This example demonstrates how to access and interact with an enum's definition,
# list its members, and ensure type correctness through casting.'''


unreal.FixedArray
Purpose: The unreal.FixedArray class is designed to represent fixed-size array instances that are exposed to Python by Unreal Engine. Unlike dynamically sized arrays (unreal.Array), FixedArray is used for arrays whose size does not change over their lifetime, mirroring the behavior and constraints of fixed-size arrays in C++. This class provides Python scripts with the ability to interact with these fixed arrays, allowing for operations such as copying and type casting while maintaining the fixed-size nature of the underlying data.

Key Methods:

__copy__() / copy(): Creates a shallow copy of the Unreal fixed-array, enabling duplication of the array's contents while preserving the original array unchanged.
cast(type, obj) → FixedArray: Casts the given Python object to the specific Unreal fixed-array type represented by this FixedArray class. This method ensures that the Python object is treated as the correct fixed-array type, which is particularly useful when dealing with generic or dynamically typed data that needs to be operated on as a fixed array.
Example Usage:

'''python

# Example: Interacting with an Unreal Engine fixed-size array in Python

# Assuming there's an Unreal Engine fixed-array named "MyFixedArray" exposed to Python
# and it is a fixed-size array of integers with a predefined size.

# Creating a FixedArray instance (assuming it can be directly instantiated or obtained from an Unreal object)
fixed_array = unreal.FixedArray([1, 2, 3, 4, 5])

# Copying the fixed array
fixed_array_copy = fixed_array.copy()

# Casting a generic object to a FixedArray (useful in dynamic contexts)
# Assume 'some_object' is a variable that is expected to be an instance of a FixedArray
casted_array = unreal.FixedArray.cast(int, some_object)

# Note: This example assumes the ability to directly instantiate `unreal.FixedArray` which
# might not directly correspond to how fixed arrays are typically created or obtained in Unreal Engine.
# Fixed arrays are often exposed as properties of other objects rather than being standalone entities.

print(f"Original FixedArray: {fixed_array}")
print(f"Copied FixedArray: {fixed_array_copy}")'''

unreal.FunctionDef
Purpose: The unreal.FunctionDef class is utilized within Unreal Engine's Python API to define UFunction fields from Python scripts. UFunctions are a cornerstone of Unreal Engine's architecture, allowing for the declaration of functions that can be exposed to the editor, Blueprints, and other parts of the engine. By using unreal.FunctionDef, developers can create custom functions in Python that integrate seamlessly with the Unreal Engine ecosystem, including support for Blueprint callable functions, replication, and event binding.
Example Usage:

Defining a custom UFunction in Python requires a combination of unreal.FunctionDef and decorators to specify the function's properties and its integration points with Unreal Engine. Here's a conceptual example:

'''python

import unreal

# Assuming we have a custom Actor class defined in Python
class MyCustomActor(unreal.Actor):

    # Define a UFunction that can be called from Blueprints
    @unreal.ufunction(ret=int, params=[int, int], meta=dict(Category="CustomMath", Tooltip="Adds two integers"))
    def add_integers(self, a, b):
        """
        A custom function defined in Python that adds two integers.
        
        Args:
            a (int): The first integer.
            b (int): The second integer.
        
        Returns:
            int: The sum of a and b.
        """
        return a + b'''

unreal.Map
Purpose: The unreal.Map class represents a map (or dictionary) data structure exposed to Python by Unreal Engine. It mimics the behavior of Python's native dictionaries but is integrated with Unreal Engine's type system, allowing for the use of Unreal Engine types as keys and values. This class facilitates operations such as storing, retrieving, and manipulating key-value pairs within the context of Unreal Engine projects, making it suitable for data storage, configuration settings, and other scenarios where associative arrays are useful.

Key Methods:

__copy__() / copy(): Creates a shallow copy of the Unreal map, enabling duplication of the map's contents.
cast(key, value, obj): Casts the given Python object to the specific Unreal map type, based on the specified key and value types. This method is useful for ensuring type safety when working with maps that are expected to contain specific Unreal Engine types.
clear(): Removes all items from the map, resulting in an empty map.
fromkeys(sequence, value=None): Creates a new Unreal map with keys from the provided sequence and all values set to the given default value. The key and value types are inferred based on the provided arguments.
get(key, default=None): Retrieves the value associated with the specified key, or returns the default value if the key is not found in the map.
items(): Returns a view object that displays a list of the map's key-value pair tuples, similar to Python's dictionary .items() method.
keys(): Returns a view object that displays a list of the map's keys, akin to Python's dictionary .keys() method.
pop(key, default=None): Removes the specified key from the map and returns its value. If the key is not found, it returns the default value if provided, or raises a KeyError otherwise.
popitem(): Removes and returns an arbitrary key-value pair from the map. If the map is empty, a KeyError is raised.
setdefault(key, default=None): Inserts the key with the specified value into the map if the key is not already present. If the key exists, it returns its associated value.
update(...): Updates the map with key-value pairs from another map or an iterable of key-value pairs, similar to Python's dictionary .update() method.
values(): Returns a view object that displays a list of the map's values, similar to Python's dictionary .values() method.
Example Usage:

'''python

# Example: Creating and manipulating an Unreal Map

# Create an Unreal Map instance
my_map = unreal.Map()
my_map["key1"] = "value1"
my_map["key2"] = "value2"

# Copy the map
my_map_copy = my_map.copy()

# Update the map with new key-value pairs
my_map.update({"key3": "value3", "key4": "value4"})

# Get a value with a default
value = my_map.get("key5", "default_value")

# Remove a key and get its value
popped_value = my_map.pop("key1", "Not Found")

# Iterate over keys and values
for key, value in my_map.items():
    print(f"{key}: {value}")

# Clear the map
my_map.clear()

print(f"Map after clearing: {list(my_map.items())}")'''

unreal.MulticastDelegateBase
Purpose: The unreal.MulticastDelegateBase class represents multicast delegates in Unreal Engine that are exposed to Python. Multicast delegates are a powerful feature of Unreal Engine that allow multiple functions to be bound to a single delegate, enabling them to all be called at once when the delegate is invoked. This is particularly useful for event-driven programming where multiple subscribers may want to respond to a single event happening, such as a player's health changing or an environmental trigger being activated.

Key Methods:

__copy__() / copy(): Creates a shallow copy of the multicast delegate, allowing for the duplication of delegate bindings.
add_callable(callable): Adds a Python callable to the delegate's invocation list, allowing it to be called when the delegate is broadcast.
add_callable_unique(callable): Adds a Python callable to the invocation list only if it is not already present, ensuring unique bindings.
add_function(obj, name): Binds a named Unreal Engine function on a given object to the delegate, allowing it to be called upon delegate invocation.
add_function_unique(obj, name): Adds a unique binding for a named Unreal Engine function on a given object, avoiding duplicate bindings.
broadcast(...): Invokes the delegate, calling all bound functions in the invocation list.
cast(object): Casts the given object to this Unreal delegate type, ensuring type safety.
clear(): Clears all bindings from the delegate's invocation list, effectively unbinding all functions.
contains_callable(callable): Checks if a Python callable is in the delegate's invocation list.
contains_function(obj, name): Determines whether a named Unreal function on a given object is bound to the delegate.
is_bound(): Checks if the delegate has any bindings in its invocation list.
remove_callable(callable): Removes a Python callable from the delegate's invocation list.
remove_function(obj, name): Unbinds a named Unreal function on a given object from the delegate.
remove_object(obj): Removes all bindings associated with a specific object from the delegate's invocation list.
Example Usage:

'''python

# Example: Using unreal.MulticastDelegateBase to bind and invoke Python callables

# Define a Python function to respond to an event
def on_custom_event():
    print("Custom event triggered.")

# Assuming `my_multicast_delegate` is an instance of an Unreal multicast delegate
# Bind the Python function to the multicast delegate
my_multicast_delegate.add_callable(on_custom_event)

# Broadcast the delegate to trigger all bound functions
my_multicast_delegate.broadcast()

# Remove the callable from the multicast delegate
my_multicast_delegate.remove_callable(on_custom_event)'''

unreal.Name
Purpose: The unreal.Name class represents the concept of names within Unreal Engine, which are used extensively as identifiers for objects, properties, and more. Unlike simple strings, Name instances in Unreal are optimized for performance, particularly for comparison operations, and are used where identifiers might be compared frequently. This class provides a Python interface to Unreal Engine's FName type, allowing scripts to safely and efficiently handle these name identifiers.

Key Methods:

cast(object) → Name: Converts a given Python object to an unreal.Name instance, ensuring that it can be used wherever FName is expected in Unreal Engine's API. This method is crucial for interacting with parts of the Unreal Engine API that require FName parameters.
is_none() → bool: Checks if the unreal.Name instance is equivalent to NAME_None, Unreal Engine's way of representing an empty or unset name. This method is useful for validation checks to avoid processing on unnamed or uninitialized identifiers.
is_valid() → bool: Determines whether the unreal.Name instance represents a valid name. This can be used to ensure that operations using the name are likely to succeed, avoiding errors related to invalid identifiers.
Example Usage:

'''python

# Example: Working with unreal.Name

# Casting a string to unreal.Name
my_name = unreal.Name.cast("MyIdentifier")

# Checking if the name is valid and not 'None'
if my_name.is_valid() and not my_name.is_none():
    print(f"The name '{my_name}' is valid and not None.")
else:
    print("The name is invalid or None.")

# Using unreal.Name in an API call that requires an FName
# Assuming there's a function that requires an FName (unreal.Name in Python)
# unreal.some_function_requiring_fname(my_name)'''

unreal.ObjectIterator
Purpose: The unreal.ObjectIterator class is designed for iterating over instances of Unreal Objects within the current Unreal Engine project or environment. This iterator facilitates the traversal of all loaded objects, or those of a specific class, providing a Pythonic way to access and interact with the vast array of objects managed by Unreal Engine. It's particularly useful for scripts that need to perform operations on multiple objects, such as searching, filtering, batch processing, or simply enumerating through objects for analysis and debugging purposes.
Example Usage:

Here's a simple example illustrating how to use unreal.ObjectIterator to iterate over all actors of a specific type in the current level and perform operations on them:

'''python

# Example: Iterating over all instances of a specific actor class

# Assuming you're looking for instances of StaticMeshActor
for obj in unreal.ObjectIterator(unreal.StaticMeshActor):
    # Perform operations on each StaticMeshActor
    print(f"Found StaticMeshActor: {obj.get_name()}")

    # Example operation: Hide all StaticMeshActors
    if isinstance(obj, unreal.StaticMeshActor):  # Double-check the instance type if needed
        obj.set_actor_hidden_in_game(True)

# Note: This example assumes that the ObjectIterator is being used in an editor script
# where actors and other objects are loaded and accessible.'''

unreal.PropertyDef
Purpose: The unreal.PropertyDef class is used within Unreal Engine's Python API to define FProperty fields from Python scripts. FProperty is a fundamental part of Unreal Engine's reflection system, representing properties of objects that can be automatically serialized, replicated, and exposed to the editor and Blueprints. By using unreal.PropertyDef, developers can create custom properties for their Python-based Unreal Engine classes, facilitating data storage, configuration, and interaction that fully integrates with the engine's ecosystem.
Example Usage:

Defining custom properties in Python requires understanding how unreal.PropertyDef works in conjunction with other Unreal Engine Python API features, such as decorators, to register these properties with the engine. Here's a conceptual example:

'''python

import unreal

# Assuming we have a custom Actor class
class MyCustomActor(unreal.Actor):
    
    # Use decorators and unreal.PropertyDef to define a custom property
    @unreal.uproperty(int, meta={"ToolTip": "An example integer property.", "DisplayName": "My Int Property"})
    def my_int_property(self):
        return self._my_int_property

    @my_int_property.setter
    def my_int_property(self, value):
        self._my_int_property = value
'''

unreal.ScopedEditorTransaction
Purpose: The unreal.ScopedEditorTransaction class is designed for creating and managing editor transactions within the Unreal Engine editor through Python scripts. Transactions in Unreal Engine are used to encapsulate a series of operations or changes into a single unit that can be undone or redone as a single action. This is particularly useful for creating editor tools or scripts that modify the level, assets, or other properties in a way that integrates with the editor's undo/redo system. The ScopedEditorTransaction class leverages Python's context management capabilities to ensure that transactions are correctly started and ended, even in the case of errors.
Example Usage:

'''python

import unreal

# Example: Modifying an actor's location within a transaction
actor = unreal.EditorLevelLibrary.get_selected_level_actors()[0]

# Start a scoped transaction
with unreal.ScopedEditorTransaction("Move Actor Example") as transaction:
    # Perform the operation that we want to be able to undo
    new_location = unreal.Vector(100, 200, 300)
    actor.set_actor_location(new_location)

    # Optionally cancel the transaction if some condition is met
    # if some_condition:
    #     transaction.cancel()'''

unreal.ScopedSlowTask
Purpose: The unreal.ScopedSlowTask class is designed for creating and managing slow tasks within the Unreal Engine editor through Python. These tasks are typically long-running operations that could benefit from user feedback, such as progress bars or cancelation options. This class provides a structured way to implement these operations, integrating with Unreal Engine's task management system to display progress and handle user interactions.
Example Usage:

'''python

import unreal

# Example: Performing a long-running operation with progress feedback
num_steps = 10
description = "Processing long task..."

# Initialize a ScopedSlowTask with the number of steps and a description
with unreal.ScopedSlowTask(num_steps, description) as slow_task:
    slow_task.make_dialog(can_cancel=True)  # Optionally allow the user to cancel the task

    for step in range(num_steps):
        # Check if the user has requested cancelation
        if slow_task.should_cancel():
            break

        # Update the task's progress and description for the current step
        step_description = f"Processing step {step + 1} of {num_steps}..."
        slow_task.enter_progress_frame(1, step_description)

        # Perform the actual work for this step (placeholder for real operations)
        unreal.log(f"Completed step {step + 1}")'''

unreal.SelectedActorIterator
Purpose: The unreal.SelectedActorIterator class is specifically designed for iterating over actor instances that are currently selected in the Unreal Engine editor. This iterator provides a convenient and efficient way for scripts to access and manipulate the set of actors that a user has selected in the editor, enabling batch operations such as transformations, property adjustments, or custom scripting actions to be applied directly to those actors.
Example Usage:

'''python

import unreal

# Example: Iterating over all selected actors in the Unreal Editor and printing their names

for actor in unreal.SelectedActorIterator():
    # Perform any desired operations on each selected actor
    print(f"Selected Actor: {actor.get_name()}")

    # Example operation: Hide the selected actors
    actor.set_actor_hidden_in_game(True)'''

unreal.Set
Purpose: The unreal.Set class is designed to represent set data structures within Unreal Engine that are exposed to Python. Similar to Python's native set, unreal.Set allows for the storage of unique elements, providing a robust interface for performing set operations like unions, intersections, differences, and more. This class is particularly useful for scripts that need to manage collections of unique items within Unreal Engine, such as unique actor tags, asset identifiers, or custom game data.

Key Methods:

__copy__() / copy(): Creates a shallow copy of the Unreal set, allowing for duplication of the set's contents.
add(value): Adds a value to the set if it is not already present, ensuring the uniqueness of elements.
cast(type, obj): Casts the given Python object to the specific Unreal set type, based on the specified element type. This method is useful for type conversion and ensuring the set contains elements of the correct type.
clear(): Removes all elements from the set, resulting in an empty set.
difference(...) -> Set: Returns a new set containing elements present in the set but not in the other specified iterables.
difference_update(...) -> None: Modifies the set to contain only elements present in the set but not in the other specified iterables.
discard(value): Removes the specified value from the set if present; otherwise, does nothing.
intersection(...) -> Set: Returns a new set containing only elements that are present in both the set and the other specified iterables.
intersection_update(...) -> None: Modifies the set to contain only elements that are present in both the set and the other specified iterables.
isdisjoint(other) → bool: Checks if the set has no elements in common with another set (i.e., if their intersection is empty).
issubset(other) → bool: Checks if all elements of the set are contained in another set.
issuperset(other) → bool: Checks if the set contains all elements of another set.
pop() → value: Removes and returns an arbitrary element from the set. Raises a KeyError if the set is empty.
remove(value) → None: Removes the specified value from the set. Raises a KeyError if the value is not present.
symmetric_difference(other) -> Set: Returns a new set containing elements that are in either of the sets but not in both.
symmetric_difference_update(other) -> None: Modifies the set to contain only elements that are in either of the sets but not in both.
union(...) -> Set: Returns a new set containing all elements that are in the set or in any of the specified iterables.
update(...) -> None: Modifies the set to contain all elements that are in the set or in any of the specified iterables.
Example Usage:

'''python

import unreal

# Create an Unreal set and add some values
my_set = unreal.Set()
my_set.add("Item1")
my_set.add("Item2")

# Create another set for comparison
other_set = unreal.Set()
other_set.add("Item2")
other_set.add("Item3")

# Perform set operations
union_set = my_set.union(other_set)
intersection_set = my_set.intersection(other_set)
difference_set = my_set.difference(other_set)

# Print results
print(f"Union: {list(union_set)}")
print(f"Intersection: {list(intersection_set)}")
print(f"Difference: {list(difference_set)}")

# Remove an item and check if it's disjoint
my_set.remove("Item1")
print(f"Is disjoint with other_set: {my_set.isdisjoint(other_set)}")'''

unreal.StructBase
Purpose: The unreal.StructBase class represents the foundation for all struct instances exposed to Python by Unreal Engine. Structs in Unreal Engine are used to group related data together in a single entity, similar to C++ structs or Python tuples, but with rich integration into Unreal's type system, editor, and serialization mechanisms. StructBase provides a Python interface to these structs, allowing for the manipulation of their properties, copying, and type-safe assignment between structs.

Key Methods:

__copy__() / copy(): Creates a shallow copy of the Unreal struct, enabling the duplication of its contents without modifying the original struct.
assign(object): Assigns the values from the given object to this Unreal struct, allowing for easy copying of values between compatible structs.
cast(object) → struct: Converts a given Python object to the specific Unreal struct type represented by this StructBase class, ensuring type safety and compatibility.
get_editor_property(name) → object: Retrieves the value of a property by name, provided the property is visible in the Unreal Editor, facilitating reflection and dynamic property access.
set_editor_properties(property_info): Sets multiple properties at once using a dictionary of name-value pairs, streamlining the process of updating struct properties from Python code.
set_editor_property(name, value, notify_mode=PropertyAccessChangeNotifyMode.DEFAULT): Sets the value of a single property by name, with an optional parameter to control notification behavior, allowing for fine-grained control over property updates.
static_struct() → Struct: Returns the static struct object associated with this StructBase type, offering access to the struct's metadata, such as its fields and type information.
to_tuple() → tuple: Converts the Unreal struct into a Python tuple containing the values of its properties, simplifying the process of exporting or inspecting struct data.
Example Usage:

'''python

import unreal

# Assuming there is a custom Unreal Engine struct named "MyCustomStruct" exposed to Python
# with properties "Property1" (int) and "Property2" (string)

# Create an instance of MyCustomStruct
my_struct = unreal.MyCustomStruct(Property1=10, Property2="Hello")

# Copy the struct
struct_copy = my_struct.copy()

# Update a property value
my_struct.set_editor_property("Property1", 20)

# Retrieve and print a property value
property1_value = my_struct.get_editor_property("Property1")
print(f"Property1 Value: {property1_value}")

# Assign values from another struct
another_struct = unreal.MyCustomStruct(Property1=30, Property2="World")
my_struct.assign(another_struct)

# Convert struct to tuple
struct_tuple = my_struct.to_tuple()
print(f"Struct as Tuple: {struct_tuple}")'''

unreal.StructIterator
Purpose: The unreal.StructIterator class is designed for iterating over Unreal Engine struct types that are exposed to Python. This iterator enables developers to programmatically traverse and inspect the various struct types registered within the Unreal Engine ecosystem, facilitating operations such as dynamic type analysis, documentation generation, or the automatic creation of UI elements based on struct properties. It provides a Pythonic way to access and work with the metadata of Unreal Engine's complex data structures.
Example Usage:

'''python

import unreal

# Example: Iterating over all Unreal Engine struct types and printing their names

for struct_type in unreal.StructIterator():
    # Get the static struct object to access its properties
    static_struct = struct_type.static_struct()
    
    # Print the struct's name
    print(f"Struct Name: {static_struct.get_name()}")
    
    # Optionally, list properties of the struct
    for property in static_struct.get_properties():
        print(f"    Property Name: {property.get_name()}, Type: {property.get_property_class().get_name()}")

# This loop demonstrates how to access and display information about each struct type,
# including its properties and their types, leveraging Unreal Engine's reflection system'''

unreal.Text
Purpose: The unreal.Text class is designed to represent text data within Unreal Engine, exposed to Python. It is used to handle localized text, supporting internationalization (i18n) and localization (l10n) efforts within Unreal Engine projects. This class offers methods to work with text in ways that respect cultural norms for formatting numbers, currencies, percentages, and more. Additionally, it provides functionality for checking the state of the text, such as whether it's empty, culture-invariant, or derived from a string table.

Key Methods:

as_currency(val, code): Converts a number to a localized currency representation, using the smallest unit for the given currency code (e.g., cents for USD).
as_number(num): Converts a number to its localized textual representation according to the current culture's formatting rules.
as_percent(num): Converts a number to a localized percentage representation, formatting it according to the current culture's norms.
cast(object): Casts a given Python object to an unreal.Text instance, enabling type-safe operations with text within Unreal Engine's Python API.
format(...): Formats the text using the provided arguments, similar to Python's str.format method, but with support for localization.
is_culture_invariant(): Checks if the text is culture-invariant, meaning it does not change based on the current culture (locale).
is_empty(): Determines whether the text is empty.
is_empty_or_whitespace(): Checks if the text is either empty or consists only of whitespace characters.
is_from_string_table(): Indicates whether the text references an entry in a string table, supporting efficient localization updates.
is_transient(): Checks if the text is transient, implying it's not meant for serialization or long-term storage.
to_lower(): Converts the text to lowercase in a way that respects the current culture's casing rules.
to_upper(): Converts the text to uppercase, considering the culture-specific rules for case conversion.
Example Usage:

'''python

import unreal

# Converting a number to a localized text representation
number_text = unreal.Text.as_number(123456)
print(f"Localized Number: {number_text}")

# Formatting text with localized currency representation
currency_text = unreal.Text.as_currency(123456, "USD")
print(f"Localized Currency: {currency_text}")

# Creating and formatting text with placeholders
formatted_text = unreal.Text.format("Hello, {name}! Welcome to Unreal Engine.", name="John Doe")
print(f"Formatted Text: {formatted_text}")

# Checking if the text is empty and converting to uppercase
if not formatted_text.is_empty():
    uppercase_text = formatted_text.to_upper()
    print(f"Uppercase Text: {uppercase_text}")'''


unreal.ValueDef
Purpose: The unreal.ValueDef class is designed for defining constant values from Python within the Unreal Engine environment. This facility allows Python scripts to declare constants that can then be utilized throughout the Unreal Engine project, ensuring that key values remain consistent and are easily maintainable. This is particularly useful for game development scenarios where certain values, such as gameplay parameters, need to be accessed across different scripts and Blueprints.
Example Usage:

While there's no direct class named unreal.ValueDef in the documentation available up to April 2023, defining constant values in Python for use in Unreal Engine typically involves setting global variables in a module. These constants can then be accessed throughout the Python scripts in the project. Here’s a conceptual approach to defining and using constants in an Unreal Engine Python script:

'''python

# Define constants in a Python module
GAMEPLAY_PARAMETER = 100
LEVEL_NAME = "MainLevel"

# Usage in another part of the project
import unreal

# Accessing the constant value
print(f"Gameplay Parameter: {GAMEPLAY_PARAMETER}")
print(f"Level Name: {LEVEL_NAME}")

# Example of using the constant in an Unreal Engine operation
actor = unreal.find_actor_by_name(unreal.get_all_level_actors(), LEVEL_NAME)
if actor:
    actor.set_actor_scale3d(unreal.Vector(GAMEPLAY_PARAMETER, GAMEPLAY_PARAMETER, GAMEPLAY_PARAMETER))'''


unreal.AbcCompressionSettings
The unreal.AbcCompressionSettings class encapsulates settings for compressing Alembic (.abc) files within Unreal Engine, especially when importing these files using the Alembic Importer plugin. These settings control how the imported Alembic data is processed, optimized, and potentially compressed for use within Unreal Engine projects. Here's a breakdown of its properties and functionalities:

Properties:
bake_matrix_animation (bool): Determines whether animations that only involve transformations (translation, rotation, scale) should be converted into vertex animations. This can be useful when the animation is complex, but you want to preserve it accurately in a form that Unreal Engine can utilize more efficiently.

base_calculation_type (BaseCalculationType): Specifies the method used to determine the number of bases (key morph targets) to be generated from the Alembic file. The bases can be calculated based on a percentage of the total or a fixed maximum number.

max_number_of_bases (int32): Sets a fixed number of morph targets to generate from the Alembic data. This is used when base_calculation_type is set to generate a fixed number of bases.

merge_meshes (bool): Indicates whether individual meshes found in the Alembic file should be merged. Merging can be beneficial for compression and performance but might not be desirable for all use cases, especially when individual mesh manipulation is required.

minimum_number_of_vertex_influence_percentage (float): Defines the minimum percentage of vertices that must be influenced by a morph target for it to be considered valid. This setting helps filter out morph targets that have minimal impact, reducing the amount of data needed to be stored.

percentage_of_total_bases (float): When the base_calculation_type is set to be percentage-based, this value determines what percentage of the calculated total number of bases should be generated as morph targets.

Usage Example:
While the class is primarily used internally by the engine during the import process, understanding how to configure these settings can significantly impact the performance and quality of Alembic animations in Unreal Engine. If you're scripting the import process or providing a UI for custom import settings, you might adjust these properties to balance between animation fidelity and performance.

'''python

import unreal

# Example configuration for AbcCompressionSettings
compression_settings = unreal.AbcCompressionSettings(
    merge_meshes=True,
    bake_matrix_animation=False,
    base_calculation_type=unreal.BaseCalculationType.PERCENTAGE_BASED,
    percentage_of_total_bases=50.0,
    max_number_of_bases=100,
    minimum_number_of_vertex_influence_percentage=0.5
)

# Assuming you have a function that imports Alembic files where you can pass these settings
# unreal.import_alembic_file("path/to/your/file.abc", compression_settings)'''

unreal.AbcConversionSettings
The unreal.AbcConversionSettings class encapsulates the settings used for converting Alembic (.abc) files upon import into Unreal Engine, with specific focus on transformation adjustments like scaling, rotation, and texture coordinate flipping. These settings allow for the customization of how Alembic content is integrated into an Unreal Engine project, ensuring that assets from different 3D content creation tools fit seamlessly within the engine's environment and coordinate system.

Properties:
flip_u (bool): Determines whether the U channel of the texture coordinates should be flipped. This can be necessary when the source content's UV mapping does not match Unreal Engine's expected UV layout.

flip_v (bool): Controls the flipping of the V channel in the texture coordinates. Since different software packages have different conventions for the V axis in UV mappings, this option ensures compatibility with Unreal Engine's UV coordinate system.

preset (AbcConversionPreset): Specifies a preset for the conversion settings, which can be tailored to match the conventions of popular 3D content creation tools like Maya, 3ds Max, etc. This preset automatically configures other settings to match the expected transformations for content coming from the chosen software.

rotation (Vector): Defines a rotation in Euler angles (pitch, yaw, roll) that should be applied to the imported Alembic content. This allows for the adjustment of asset orientation to fit the Unreal Engine scene's coordinate system.

scale (Vector): Specifies the scale factors to be applied to the imported content. This is critical for ensuring that the asset sizes are consistent with Unreal Engine's world scale and other assets within the project.

Usage Example:
Configuring and applying AbcConversionSettings is typically part of the Alembic import process. While direct usage in script might be limited to specific import automation tasks, understanding how to adjust these settings is crucial for correctly importing Alembic files.

'''python

import unreal

# Create an instance of AbcConversionSettings with customized import settings
conversion_settings = unreal.AbcConversionSettings(
    preset=unreal.AbcConversionPreset.MAYA,
    flip_u=False,
    flip_v=True,
    scale=unreal.Vector(1.0, -1.0, 1.0),
    rotation=unreal.Vector(0.0, 0.0, 0.0)
)

# Assuming there's a function or procedure to import Alembic files where you can specify conversion settings
# This might be part of a custom import script or automation tool within an Unreal Engine project
# unreal.import_alembic("path/to/your/alembic.abc", conversion_settings)'''

unreal.AbcGeometryCacheSettings 
The unreal.AbcGeometryCacheSettings class encapsulates a set of import settings tailored for handling geometry cache data from Alembic (.abc) files in Unreal Engine. These settings allow for significant customization of the import process, focusing on performance optimization, data compression, and animation handling. Here's an overview of each property and its impact on the import process:

Properties:
apply_constant_topology_optimizations (bool): This setting forces the importer to apply topology optimizations only once, rather than dynamically based on the preprocessor's decisions. While this can ensure consistent motion blur effects for meshes with constant topology, it may cause issues with some meshes.

compressed_position_precision (float): Specifies the precision used for compressing vertex positions. A lower value means better accuracy but less compression, while a higher value results in more aggressive compression at the cost of precision.

compressed_texture_coordinates_number_of_bits (int32): Determines the bit precision for compressing texture coordinates. Higher values offer better results with less compression; lower values increase compression at the expense of accuracy.

flatten_tracks (bool): Indicates whether all vertex animations should be merged into a single track. This can simplify the animation data but might reduce flexibility in handling individual animations.

motion_vectors (AbcGeometryCacheMotionVectorsImport): Specifies how motion vectors are imported, which are essential for accurate motion blur effects. Options include importing no motion vectors, importing them for vertices, or importing them for positions and normals.

optimize_index_buffers (bool): Enables or disables optimization of index buffers for each unique frame. This optimization aims to improve GPU cache coherency but is a resource-intensive process, so it's generally recommended to be turned off unless specifically needed.

store_imported_vertex_numbers (bool): When enabled, stores the original vertex numbers from the DCC tool. This is useful for debugging or when precise mapping of vertex data from the source file is required.

Example Usage:
While unreal.AbcGeometryCacheSettings is typically used internally during the Alembic import process, understanding these settings is crucial when scripting custom imports or adjusting import parameters for optimal performance and fidelity.

'''python

import unreal

# Create a new instance of AbcGeometryCacheSettings with custom settings
geometry_cache_settings = unreal.AbcGeometryCacheSettings(
    flatten_tracks=True,
    store_imported_vertex_numbers=False,
    apply_constant_topology_optimizations=False,
    motion_vectors=unreal.AbcGeometryCacheMotionVectorsImport.NO_MOTION_VECTORS,
    optimize_index_buffers=False,
    compressed_position_precision=0.01,
    compressed_texture_coordinates_number_of_bits=10
)

# Assuming you have a function to import Alembic files where you can specify these settings
# This function would use geometry_cache_settings as part of the import process'''

unreal.AbcMaterialSettings
The unreal.AbcMaterialSettings class is specifically designed to manage material-related settings during the import of Alembic (.abc) files into Unreal Engine. These settings determine how Unreal Engine should handle materials that are associated with the geometry in the Alembic file, particularly in relation to Face Set names defined within the file. Face Sets in Alembic files are often used to assign different materials to different parts of a mesh.

Properties:
create_materials (bool): Specifies whether Unreal Engine should automatically create new materials for each unique Face Set name found within the Alembic file. This is useful when importing models that require distinct materials for different parts of the mesh but do not have those materials defined within the file. Note that this option will only work if the Alembic file includes Face Set names.

find_materials (bool): Determines whether Unreal Engine should attempt to match and assign existing materials based on the Face Set names found within the Alembic file. This option is useful for projects where materials corresponding to the Face Set names already exist within the Unreal Engine project, allowing for automatic material assignment during import.

Example Usage:
When importing an Alembic file into Unreal Engine using Python scripting, you might configure the AbcMaterialSettings to automatically create or find materials based on the Face Sets defined in the file. Here's how you might set up these material settings:

'''python

import unreal

# Configuration for importing an Alembic file with specific material settings
material_settings = unreal.AbcMaterialSettings(create_materials=True, find_materials=False)

# Assuming there's a function to import an Alembic file where you can specify material settings
# This function would utilize the configured material_settings for the import process
# The exact function to import Alembic files might vary or need to be implemented as part of the import'''

unreal.AbcNormalGenerationSettings
The unreal.AbcNormalGenerationSettings class provides a set of parameters for controlling the generation and processing of normals (and optionally tangents) when importing geometry from Alembic (.abc) files into Unreal Engine. This is crucial for ensuring that the visual appearance of meshes, particularly how they interact with lighting, matches the artistic intent or source material. Correctly setting these parameters can significantly impact the quality and performance of the imported geometry within Unreal Engine projects.

Properties:
force_one_smoothing_group_per_object (bool): If enabled, this setting forces each object to use a single smoothing group, effectively smoothing all normals across the object. This can be useful for objects that should appear as entirely smooth but might bypass the need for more nuanced smoothing group calculations.

hard_edge_angle_threshold (float): This threshold determines the angle at which an edge between two faces is considered "hard" (creating a sharp edge) versus "soft" (smoothly interpolated). Values closer to 0 create more smoothing, whereas values closer to 1 result in more hard edges.

ignore_degenerate_triangles (bool): When enabled, degenerate triangles (triangles with very small area or invalid configurations) are ignored in the normal (and tangent) computation process. This helps avoid visual artifacts and issues in mesh geometry that might arise from problematic geometry.

recompute_normals (bool): If set to true, this option forces the recomputation of normals for the imported geometry, disregarding any normal data that might be included in the Alembic file. This is useful when the source normals are incorrect or unsuitable for Unreal Engine's rendering system.

skip_computing_tangents (bool): This option, when enabled, skips the computation of tangents for the geometry cache. While skipping tangent computation can improve performance, especially for streaming, it may lead to visual artifacts in materials relying on normal mapping.

Example Usage:
Here's a hypothetical example of how unreal.AbcNormalGenerationSettings might be configured for an Alembic import process, focusing on achieving a balance between visual quality and performance:

'''python

import unreal

# Configure normal generation settings for Alembic import
normal_gen_settings = unreal.AbcNormalGenerationSettings(
    force_one_smoothing_group_per_object=False,
    hard_edge_angle_threshold=0.5,  # Intermediate value for a balance between hard and soft edges
    recompute_normals=True,  # Ensure normals are suitable for Unreal Engine
    ignore_degenerate_triangles=True,  # Avoid issues from problematic geometry
    skip_computing_tangents=False  # Compute tangents for better material appearance
)

# Assuming these settings are part of a larger Alembic import process
# You would pass `normal_gen_settings` to the relevant import function or process'''

unreal.AbcSamplingSettings 
The unreal.AbcSamplingSettings class is designed to configure how animations are sampled during the import of Alembic (.abc) files into Unreal Engine. These settings allow for precise control over which frames of the animation are imported and how the sampling is performed, optimizing both the quality and the size of the imported data according to specific project needs.

Properties:
frame_end (int32): Specifies the ending frame index for sampling the animation. This allows you to limit the import to a specific portion of the animation.

frame_start (int32): Defines the starting frame index from which to begin sampling the animation, enabling you to skip initial frames that may not be needed.

frame_steps (int32): Determines the steps between frames when sampling the animation. A value greater than 1 can be used to skip frames, reducing the total number of samples and potentially decreasing file size.

sampling_type (AlembicSamplingType): Indicates the type of sampling performed while importing the animation. Options include per-frame sampling, uniform sampling over time, or no sampling.

skip_empty (bool): If set to true, empty (pre-roll) frames at the beginning of the animation will be skipped, starting the import at the first frame that contains data.

time_steps (float): Specifies the time interval between samples when using time-based sampling. This is an alternative to frame-based sampling and allows for consistent temporal spacing between samples.

Example Usage:
When importing an Alembic file, these sampling settings can be adjusted to match the requirements of your project, whether you're looking to maintain high fidelity by sampling every frame or to reduce the imported animation's complexity and size by sampling less frequently.

'''python

import unreal

# Configure sampling settings for Alembic import
sampling_settings = unreal.AbcSamplingSettings(
    sampling_type=unreal.AlembicSamplingType.PER_FRAME,
    frame_steps=1,
    time_steps=0.0,
    frame_start=0,
    frame_end=100,
    skip_empty=True
)

# Assuming these settings are part of a larger Alembic import process
# You would pass `sampling_settings` to the relevant import function or process
# This might be particularly useful in custom import scripts or tools within Unreal Engine projects'''

unreal.AbcStaticMeshSettings
The unreal.AbcStaticMeshSettings class configures import settings specifically for Static Meshes when importing Alembic (.abc) files into Unreal Engine. These settings allow users to control how the geometry from Alembic files is processed and integrated into the Unreal project as Static Mesh assets. Understanding and correctly configuring these settings can significantly impact the usability, performance, and visual fidelity of imported meshes.

Properties:
generate_lightmap_u_vs (bool): Determines whether or not lightmap UVs should be automatically generated for the imported static meshes. Generating lightmap UVs is essential for proper light baking on static geometry but can increase import time. It's useful when the source file doesn't contain UVs suitable for lightmapping.

merge_meshes (bool): Controls whether all the static meshes found in the Alembic file should be merged into a single Static Mesh asset upon import. Merging can simplify scene hierarchy and potentially improve performance by reducing draw calls, but it might lead to issues like overlapping UVs, which are problematic for texturing and lightmap generation.

propagate_matrix_transformations (bool): If enabled, any matrix transformations (such as translations, rotations, and scaling) that are defined in the Alembic file will be applied directly to the mesh geometry during import. This ensures that the meshes appear in Unreal with the correct orientation, position, and scale but can make further adjustments within Unreal more complex.

Example Usage:
When importing an Alembic file containing static mesh data, you might want to merge all meshes into a single asset, ensure that matrix transformations are applied to maintain correct positioning, and generate lightmap UVs for lighting calculations:

'''python

import unreal

# Configure static mesh settings for Alembic import
static_mesh_settings = unreal.AbcStaticMeshSettings(
    merge_meshes=True,
    propagate_matrix_transformations=True,
    generate_lightmap_u_vs=True
)'''

AbilityEndedData
The AbilityEndedData struct specifically holds information about an ability that has just ended. This can include details such as whether the ability was canceled, completed its execution successfully, or other end conditions. It's typically used within the system to notify other parts of the game, such as UI elements or AI decision-making components, that an ability has finished its operation, allowing them to respond accordingly.

Key Components
While the specific properties of AbilityEndedData were not detailed in the prompt, a struct of this nature might typically include:

Ability Reference: A reference to the ability instance that has ended.
WasCancelled: A boolean indicating whether the ability was canceled before it could complete naturally.
ActivationInfo: Information about how the ability was activated, which could include the source character, target(s), and any relevant context or parameters that were used when the ability was initiated.
Example Usage
In a gameplay scenario, AbilityEndedData might be used in a callback or event listener that is triggered when an ability concludes. Here's a conceptual example of how it might be used in a game:

'''python

def on_ability_ended(ability_ended_data):
    if ability_ended_data.WasCancelled:
        print(f"The ability {ability_ended_data.AbilityReference.Name} was canceled.")
    else:
        print(f"The ability {ability_ended_data.AbilityReference.Name} has completed.")'''


