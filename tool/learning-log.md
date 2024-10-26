# Tool Learning Log

## Tool: **Unity**

## Project: **The New Settlement**

---

### 10/02/24: [Unity tutorial for Beginners](https://www.youtube.com/watch?v=XtQMytORBmM)
* Unity User Interface
  * Top left: hierarchy- Contains what happens in the scene, drag sprites up to create game object 
  * Bottom: project- game elements such as sprites and scripts, usually imorted images
  * Top middle: scene- Basically the preview for what happens in the scene, select game view to see how the game will look like from user perspective
  * Left: Inspector- Manipulating game objects to perform actions and name the objects

### 10/14/24: [W3 Schools](https://www.w3schools.com/cs/index.php)
* Created a codespace by following the instructions on Get Started section and got this:
```C#
namespace HelloWorld
{
    internal class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```
Tried to replicate what w3school shows by copying `Console.WriteLine(Value);` but showed error and told me to declare a value to output it:
```C#
namespace HelloWorld
{
    internal class Program
    {
        private const string Value = "Hi";

        static void Main(string[] args)
        {
            Console.WriteLine(Value);
        }
    }
}
```
Notes so far:
* `Console.WriteLine();` is like `System.out.println();`
* C# is very similar to java since it is object-oriented 

### 10/26/24: [Godot Showcase](https://godotengine.org/showcase/), [First game in Godot](https://www.youtube.com/watch?v=iwGVLiFL-Lw)
* Pros of switching to Godot:
 * Godot is very good for small companies or solo dev while Unity is mostly for game industry and big companies
 * Most people find Unity annoying while Godot is a lot more easier and more fun to code in
 * Skills are easily transferrable
 * everybody is using it so we can ask when need help
 * tutorials are much more recent


<!-- 
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
-->
