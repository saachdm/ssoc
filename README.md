## Sayid's Summer of Code 2024 (SSoC 2024)

A project-based initiative to learn C, re-learn Git, software development in automotive engineering, and upskilling in general.

I plan to build my own "simulation" system using Python for the user interface, simulation controller, and dynamics model. A program written in C then serves as
the "controller" of the dynamics. Since this project is quite close to my domain expertise, I am comfortable and excited with the problem so that I can prioritize
the C, Python, and software development aspect more. 

The main idea is to develop a C-based controller that roughly imitates how a controller works in a real vehicle.
The dynamics are governed by a separate Python-based script (well because I am much more familiar with Python so debugging the dynamics part will be much easier).
Then a simulation controller with Python will handle the "tick-tock" of the simulation time and the UDP communication between the dynamics and the controller. A separate Python-based script will
serve as the user interface that receives real-time input from the user.

General plan:

| Stage | Title | Desc |
|----------|----------|----------|
| 1 | Basics of C   | Learn the basics of C |
| 2 | Environment Setup and testing #1 | Developing the architecture of the entire simulation process for 1-DOF simulation|
| 3 | Longitudinal dynamics (Adaptive Cruise Control and ABS) | 1-DOF simulation with simple point-mass dynamics. The vehicle is commanded to follow an agent in front. The input to the simulation is the speed of the agent. ABS is included |
| 4 | Environment setup and testing #2 | Extending the architecture to accommodate 3-DOF simulation (longitudinal, lateral, and pitching motion) |
| 5 | Lateral dynamics (Lane-keeping assist) | 2-DOF simulation with a simple bicycle model. The vehicle is commanded to change lanes |
| 6 | Powertrain dynamics and Pitching dynamics | Extending the model to include the powertrain dynamics (a delay) and pitching dynamics |
| 7 | Long+Lat dynamics (ACC+lane keeping assist) | Extending the model to be able to perform ACC and maintain the same lane as the agent |
| 8 | Parameter estimation (RLS) | Implementing Kalman Filter-based Recursive Least Squares (RLS) to estimate center of gravity |

Each stage (2nd stage onwards) will be tested and documented through my blog (https://schmunt.wordpress.com). The initial architecture development (2nd stage) focus on the practicality so that
I can try the "bare minimum" of the simulation system. The second architecture development (4th stage) will focus on the scalability and the extendability of the system. I am sure that the second stage will be the most
demanding of them all :)

Hopefully, this project can be finished by the end of summer 2024.
### Usage
Compile the udpserver.c and run main.py. I haven't done any "streamlining" on this part yet.

### Architecture #1
The initial plan of the architecture is as follow:
![architecture_1](https://github.com/saachdm/cpsil/assets/20472912/8bbfb99a-9004-4646-ac2d-73cc85113730)

On the C-side, sensor readings and input to the dynamic system will use a "publisher-subscriber" method towards the C server. The user in the C-side then can focus on processing the "subscribed" values and "publishing" the processed values without worrying about the "backend" side of the C. The entire C loop will run everytime the C server receives a message from the Python server.

### Current update 25-06-2024
The minimum viable product is ready and it should be able to "capture" the important features. The plotter is still a simple matplotlib update mechanism, models are still limited to longitudinal, and lots of things to explore on the C side. Next development will focus on a more robust and modular model, adding driveline model, as well as implementing a rudimentary "bicycle" model for lateral motion.
