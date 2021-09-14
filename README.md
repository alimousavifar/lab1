---
layout: lab
title:  Lab 1 - C# Review
date:   2020-08-30 12:00:00
authors: [Ali Mousavifar, C. Antonio Sánchez]
categories: [labs, C#]
usemath: true

---

# Lab 1 -- C# Review

In this lab, we will get some practice with C\#\# and object oriented programming.  Our test-case is a simplified 1D car simulator.  To help get you started, some of the code is posted on GitHub [here (https://github.com/alimousavifar/lab1)](https://github.com/alimousavifar/lab1).

Feel free to discuss approaches and solutions with your classmates, but labs are to be completed individually.  Each student is expected to be able to answer questions about the content, describe their work, and be able to reproduce their code (or parts thereof).



## Part 1: Namespaces

Namespaces allow a programmer to group relevant code together and prevent *name collisions* -- two different methods with the exact same name -- by providing a *namespace scope*.  Methods and variables within the namespace can be referenced from outside by using the special namespace prefix.

### Q1:  A mini 1D physics library
In this section, we will create a small 1D physics library and simulator. 

Start by creating a new Solution in Visual Studio (VS) and name it CarSimulator (the namespace CarSimulator will automatically be assigned to this Program Class). A Program class which includes a main method will be created by VS. The main program is where the simulator runs. We will provide more details later. 

Now, create a new file of type Empty Class and name it Physics1D under CarSimulator project. We will define and implement the methods which are used by main program calls


```c#
using System;
namespace CarSimulator
{
    public class Physics1D
    {
	// Implement the methods

     }	
}
```


Add the following functions inside the Physics1D:

```c#
public static double compute_position(double x0, double v, double dt){ // to do}
public static double compute_velocity(double v0, double a, double dt){ // to do}
public static double compute_velocity(double x0, double t0, double x1, double t1){ // to do}
public static double compute_acceleration(double v0, double t0, double v1, double t1){ // to do}
public static double compute_acceleration(double f, double m){ // to do}
```
Note that we use static methods so that instantiation of Class Physics1D is not required to use these methods. Also note that these methods will be used in the Program Class and should be accessible to that class, i,.e public. 

In our Physics1D Class, we have a couple pairs of functions with the exact same name but a different number of arguments.  This is called *function overloading*, and is allowed in C## as long as either the number of arguments or the types of the arguments differ.  The CLR will figure out which function to use based on what you call it with.

If you need to brush-up on your 1D mechanics: position, velocity, acceleration, and force are related through
$$v = \frac{dx}{dt} \approx \frac{x_1-x_0}{t_1-t_0}$$

$$a = \frac{dv}{dt} \approx \frac{v_1-v_0}{t_1-t_0}$$

$$f = m a$$

If you are unsure about reading the equations above, you may use [quicklatex](https://quicklatex.com/) to render the equations. By re-arranging the above approximations, and by taking $$\{v, a, f\}$$ to be evaluated at time $$t_0$$, we can estimate the "next" position and velocity based on the current state $$\{x_0, v_0, a_0\}$$.  


### Q2: A simple car simulator

Next, we implement the main method of the Program class which we created in Q1. We are going to use our mini Physics1D library to simulate a car driving down a straight road.  

```c#
using System;

namespace CarSimulator
{
    class Program
    {
        static void Main(string[] args)
        {
            
            // read in car mass
            Console.WriteLine("Enter the mass of the car (kg): ");
            double mass;
            mass = Convert.ToDouble(Console.ReadLine());

            // read in engine force
            Console.WriteLine("Enter the net force of the engine (N): ");
            double engine_force;
            engine_force = Convert.ToDouble(Console.ReadLine());

            // read in drag area coefficient
            Console.WriteLine("Enter the car's drag area (m^2): ");
            double drag_area;
            drag_area = Convert.ToDouble(Console.ReadLine());

            // read in time step
            Console.WriteLine("Enter the simulation time step (s): ");
            double dt;
            dt = Convert.ToDouble(Console.ReadLine());

            // read in total number of simulation steps
            Console.WriteLine("Enter the number of time steps (int): ");
            int N;
            N = Convert.ToInt32(Console.ReadLine());

            // initialize the car's state
            double x0 = 0;  // initial position
            double v = 0;  // initial velocity
            double t = 0;  // initial time
            double fd, x1, a; // drag force and secondary position and acceleration
         

            // run the simulation
            for (int i = 0; i < N; ++i)
            {
                // TODO: COMPUTE UPDATED STATE HERE

                 t += dt;  // increment time
                
                // print the time and current state
                Console.WriteLine("t:{0}, a:{1}, v:{2}, x1:{3}, fd:{4} ", t, a, v, x1, fd);
            }

        }
    }
}


```


Note 1: ```Console.ReadLine()``` is used to read multiple characters from the console. It is included in the ```System``` namspace.
Note 2: ```Convert.ToDouble(myString)``` and ```Convert.ToInt32(myString)``` are used to convert the string input characters to double and integer. It is included in the ```System``` namspace.
Note 3: ```Console.WriteLine()``` is used to prompt messages on the console. It is included in the ```System``` namspace.

#### Implementing the physics

As a car picks up speed, there's a build-up of air-resistance that adds a force in the opposite direction of the car, trying to slow it down.  The drag force is computed as

$$f_d = \frac{1}{2}\rho A C_d\, v^2$$

where $$\rho$$ is the air-density, $$A$$ is the projected surface-area of the car and $$C_d$$ is the car's [*drag coefficient*](https://en.wikipedia.org/wiki/Automobile_drag_coefficient).  This drag force will counter-act the engine's drive force, causing your car to reach a top speed (or slow down if you take your foot off the gas).  A car's *drag area* is the product of the projected surface area and drag coefficient, $$A C_d$$.  Air density at sea-level is about $$\rho=1.225$$ kg/m<sup>3</sup>.  Typical drag coefficients and surface areas for a variety of vehicles can be found [here](https://en.wikipedia.org/wiki/Automobile_drag_coefficient#Drag_area) (e.g. a Toyota Prius has $$A C_d \approx 0.58$$m<sup>2</sup>).  We are going to ignore a lot of other contributing factors to a car's motion, like gear ratios and rolling friction.

To update the car's state, you will need to compute the updated force, acceleration, velocity, and position.  Use your Physics1D class library functions for this.  Remember that since your library exist in the namespace `CarSimulator`, you will need to prefix the function names with `CarSimulator.Physics1D` to access them. A reminder that the methods in Physics1D class should be public (accessible) for the main method in Program class to utilize them.  

#### Using your simulator

Compile and run your simulator.  Try it for a variety of inputs.  An average car weighs about 1500Kg and can produce about 750 N of driving force in its highest gear.  See if you can estimate a top speed.

## Part 2: Object Oriented Programming

We are now going to modify our car simulator, encapsulating some of the functionality into various classes.

### Q1: Object-Oriented implementation of the simulator

A `State` consists of a position, velocity, acceleration, and time.  We want these attributes to be **public** so they can be accessed directly.  We will also give the state a `set(...)` method so we can easily set all the attributes in one go.

A `Car` has a model name, mass, maximum engine force, and drag area.  The car will also have a `state` associated with it.  We'll give the car the following methods:
```c#
public class Car
    {
        private double mass;
        private string model;
        private double dragArea;
        private double engineForce;
        public State myCarState;

    }
```
We can represent the two classes and the relationship between them with a *class diagram* that looks like this:

![car class diagram](https://github.com/alimousavifar/Binaryfiles/blob/master/car-state_UML.png)

The car has several **private** member variables (indicated with a `-` prefix) and **public** member functions (indicated with a `+` prefix).  The state has **public** member variables and a single **public** member function.  Each car can access and modify a state, but a state cannot access or modify a car.

#### Creating the `State` class

We will start by creating the `State` class that will hold the car's position, velocity, acceleration, and time information.

1. Create a new file called `State.cs`.  In this file, implement the basic class according to the class diagram in the previous section. 
2. Create a new main program in a separate source file that creates an *instance* of a `State`.  Call the `set(...)` function then print out all the values of the state object to make sure it's working properly.  What happens if you print the values before calling `set(...)`?
3. Add a default constructor that initializes everything in the state to zero, and an overloaded constructor that allows you to initialize the member variables.

#### Creating the `Car` class

Next we will create the `Car` class, which will do most of the work for driving.

1. Create a new file called `Car.cs`.  In this file, complete the class *declaration*: declare and implement  all the necessary public/private member variables and functions.
Note that you may need more variables than what appears in the class diagram.  Since the class diagram leaves some things unspecified, it is up to you to fill in the details however you like. 
**Notes:**
  - You should use the `Physics1D` class you developed in Part 1 inside the `drive(...)` function implementation.
  - The car's state should be initialized to zero on construction

#### Creating the simulator

The simulator will be our main program that will drive our cars. 

1. Create a file named `DragRace.cs`, and add a main method that looks like this:
 
  ```c#
   public class DragRace
    {
        static void Main(string[] args)
        {

            Car myTesla = new Car("Tesla", 1500, 1000, 0.51);
            Car myPrius= new Car("Prius", 1000, 750, 0.43);
            string winner;


            // drive for 60 seconds
            double dt = 1;

            for (double t = 0; t < 60; t += dt){
		// print the time and current state
		// print who is in lead
	     }	
	}
    }		

   ```
   **Notes:** that each C# project can only have one entry point (i.e. main method), so after implementing the `DragRace.cs` and before running it, you must comment out the method ` static void Main(string[] args)` and its content in other classes, i.e. in the `Program` class at this stage. Alternatively, you can specify the main entry point you want to choose in `<StartupObject>CarSimulator.DragRace</StartupObject>` in the `CarSimulator.csproj` file and `Rebuild All` and `Run`.  Fill in the details to print out the current leader. 
   
2. Modify the program so that the race between the two cars ends when they both cross the 402.3m mark (a quarter mile) and print out the winner.


### Q2: Car Inheritance

Let's say we wanted to create a fleet of one hundred [Evo](https://www.evo.ca) cars (they are all Toyota Prius hybrids).  We *could* use our constructor and set the car properties each time.  *OR* we can create a `Prius` class with a default constructor that automatically sets the appropriate properties.  In this section, we will create a class hierarchy for our cars and set an entire fleet loose on the highway.

1. Create a car hierarchy of classes consisting of a Prius, Mazda 3, and Tesla Model 3. Each should have only a default constructor which calls the parent `Car` constructor with some appropriate values.  Since these implementations should be quite short, you can fully implement them within the header files.  For example, a Prius implementation might look like this:
 
```c#
 public class Prius : Car
    {
        public Prius() : base()
        {
           
        }
        public Prius(string model, double mass, double engineForce, double dragArea) : base(model, mass, engineForce, dragArea)
        {
          
        }
    }


```
2. Create a fourth car type, `Herbie`, that overrides the `drive(...)` function.  Herbie doesn't follow the laws of physics, so you can let it do whatever you like (e.g. ignore air resistance, or random velocities, etc...).  For Herbie to access some of the member variables in `Car` directly, those memeber variables which were private now need to change to **protected**:
```c#
public class Car
    {
        protected double mass;
	...
       
    }
```
3. Create a new program in a file named `Highway.cs`.  In the main method, we will create a fleet of 100 cars (25 of each car type).  
   **Note:** Here we create four different arrays or list of the explicit types `Prius`, `Mazda3`, `Tesla3`, and `Herbie` to store our fleet.
```c#
   // create fleet
  int fleetNumberPerType = 25;

  Tesla[] myTeslas = new Tesla[fleetNumberPerType].Select(//TO DO: Projects each element of a sequence into a new form).ToArray();
  Prius[] myPriuses = ...           
  Mazda[] myMazdas = ...
  Herbie[] myHerbies = ...
  ...
```  

Where ```Tesla[] myTesla``` defines a new array of class Tesla. You may use ```Enumerable.Select``` from ```System.Linq``` [namespace](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.select?view=netcore-3.1) to construct and initialize every element of each array (class Tesla, Prius, Mazda, Herbie) individually. Feel free to use any other efficient methods for constructing each class of array/list.

You can use lists or arrays and depending on your choice implementation of construct and initialization of the array or list can differ.  
**Notes:** that each C# project can only have one entry point (i.e. main method), so after implementing the `Highway.cs` and before running it, you must comment out the method ` static void Main(string[] args)` and its content in other classes, i.e. in the `Program` and `DragRace.cs` class at this stage. Alternatively, you can specify the main entry point you want to choose in `<StartupObject>CarSimulator.Highway.cs</StartupObject>` in the `CarSimulator.csproj` file and `Rebuild All` and `Run`. 


4. Instead of a list or array per inherited class of car (Tesla, Prius, ...), create a combined list of cars for storing the entire fleet.
   ```c#
   var myCars = new List<Car>();
   for (int i = 0; i < fleetNumberPerType; i++)
    {
	myCars.Add(new CarSimulator.Tesla());
	...
	// TO DO: Do the same for other class vehicles
    }
    // loop through the time and list to drive all the vehicles
    for (double t = 0; t < 60; t += dt)
    {
	  for (int i = 0; i < fleetNumberPerType; i++)
	    {
		// TO DO: Drive myCars list and Display the cars states acceleration, speed, position, etc
	    }
     }	
  
   ```
   Note that the List library is defined in `System.Collections.Generic` namespace.
## Submitting Your Lab

- Each lab is graded out of 5, and is due by the start of the following lab period (i.e. Lab 1 is due in 1 week).
- You must hand in your lab files to turnitin. You should have created an account as you first week assignment..
- You must commit your code to a **private** github repository and add your specific Lab Section TA user-name as collaborators to your private repository. 

