#include "HelloWorld.h"
#include <godot_cpp/core/class_db.hpp>
#include <godot_cpp/godot.hpp>
#include <godot_cpp/variant/utility_functions.hpp>
#include <godot_cpp/classes/engine.hpp>
#include <godot_cpp/classes/input.hpp>

using namespace godot;

void HelloWorld::_bind_methods()
{
    ClassDB::bind_method(D_METHOD("get_speed"), &HelloWorld::get_speed);
    ClassDB::bind_method(D_METHOD("set_speed", "speed"), &HelloWorld::set_speed);
    ClassDB::add_property("HelloWorld",PropertyInfo(Variant::FLOAT, "speed"), "set_speed", "get_speed");

    ClassDB::bind_method(D_METHOD("hello_world", "words"), &HelloWorld::hello_world);
    ADD_SIGNAL(MethodInfo("Hello_World_Signal", PropertyInfo(Variant::STRING, "data")));
}
HelloWorld::HelloWorld()
{
    if(Engine::get_singleton()->is_editor_hint())
    {
        set_process_mode(Node::ProcessMode::PROCESS_MODE_DISABLED);
    }
    UtilityFunctions::print("Hello world!");
}

HelloWorld::~HelloWorld()
{
        
}

void HelloWorld::hello_world(String words)
{
    UtilityFunctions::print("hello world! " + words);
    emit_signal("Hello_World_Signal", "data! ");
}

void HelloWorld::_process(double delta)
{
    UtilityFunctions::print("Hello from process!");
    velociity = Vector2(0.0f,0.0f);

    Input& inputSingleton = *Input::get_singleton();

    if(inputSingleton.is_action_pressed("d"))
    {
        velociity.x += 1.0f;
    }

    if(inputSingleton.is_action_pressed("a"))
    {
        velociity.x -= 1.0f;
    }

    if(inputSingleton.is_action_pressed("w"))
    {
        velociity.y -= 1.0f;
    }

    if(inputSingleton.is_action_pressed("s"))
    {
        velociity.y += 1.0f;
    }

    set_position(get_position() + (velociity * speed * delta));
}

void HelloWorld::set_speed(const double speed)
{
    this->speed = speed;
}

double HelloWorld::get_speed() const{
    return speed;
}