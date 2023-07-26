# javascript-neural-machine

Goal is a self-driving car simulation using canvas, neural network, and machine learning
Setup basic html page
Setup basic css (canvas always starts off transparent)
Bring myCanvas into your js file, and set your canvas height to window innerheight(full height of your browser), and canvas width of 200px(or more if you want)
Set ctx(context) by calling getContext of 2d on canvas
We will have a car object so add a new js file called car.js, and link it in your html(above your main.js so it has access to car.js)
Inside your car.js, create Car class, passing in x, y, width, and height in your constructor, and convert them to class properties
Create custom draw method inside Car, passing in ctx
Inside draw, Call beginPath on ctx, then call rect on ctx, passing in x minus half of width for x(centerpoint of width), y minus half of height for y(centerpoint of height), width for width, and height for height
After the above, call fill on ctx
In your main js file, set car to a new instance of Car object, passing in 100, 100, 50, 80 for x, y, width, and height values for now, then call draw on car, passing in ctx
You should now see the default black color rectangle defined by rect which is the placeholder for the car you want to simulate
After the above is done, we want to implement some controls for the 'car'
In your Car, set controls to a new instance of Controls(not yet made)
Create a new js file called controls.js and link it in your html(again above your main.js AND car.js because car.js require access to it)
In controls.js, create Controls object, and insde it set forward, left, right, reverse class properties to false initially
We will now employ keyboard listeners to check for the above directions when pressing a directional key
To do this, set a PRIVATE addkeyBoardListeners call in Controls properties(# --> private keyword)
Then, create a PRIVATE addkeyBoardListeners custom method inside Controls
Inside the above method, use arrow function for document's onkeydown, passing in the event(or e) and inside the arrow function, use a switch statement for each event(or e) key for ArrowLeft, ArrowRight, ArrowUp, and ArrowDown, setting their respective directions to true in each of their cases(match them with your class properties in Controls)
Do the same for onkeyup for your document but instead of true, set them back to false when the keys are released
Console table on this keyword after each onkeydown and onkeyup statements to debug
You should see values changing in your console for each directional key press
\*\* Remember function(event) binds 'this' to the function itself while an arrow function unbinds it and will refer 'this' to anywhere that has this keyword
Back to your Car object, add a custom update method
Inside the update method, set an if statement for each direction and hardcode the values to an increment or decrement of 2 for now depending on their direction(do it for forward and reverse only for now)
Inside your main.js file, create a custom animate function, and call update on car inside it, and move your draw call on car inside it as well, then call requestAnimationFrame, passing in animate
Call animate after
You should see smudging because the previous draws are still on the canvas
\*\* In previous projects we used clearRect to clear previous draws, but this author moves canvas.height = window.innerHeight to remove the smudging, which i believe only works because currently the car only moves up and down
Cars do not move and stop at will but move and stops at decreasing or increasing speeds
So, add a speed property in Car and set it to 0
Add an acceleration property and set it to 0.2
Inside your update method, set speed to an increment of acceleration for forward and a decrement of acceleration for reverse
Then, set y to a decrement of speed
Moving up or down will accelerate your 'car' by 0.2 forever in the direction you pressed since there are no limitations in place
Introduce some limitation properties by setting maxSpeed to 3 in Car, and friction to 0.05 in Car properties
To cap speed for forward movement, inside your update method, if speed is more than maxSpeed, speed is maxSpeed
To cap speed for reverse movement, if speed is less than half of negative maxspeed, set speed to half of negative maxspeed(negative in javascript just means the opposite because speed can never be negative in by laws of physics, half because the author wants the car to reverse slower than going forward) ----> \*\* For this i exclude the half and just allow car to move forward and in reverse at the same speed
For the car to gradually slow down to a halt, if speed is more than 0, decrement speed by friction for forward movement
If speed is less than 0, increment speed by friction for reverse movement
\*\* Moving the car slightly will cause the car to keep moving indefinitely because of friction, causing speed to never be 0. To fix this, just set a condition when the absolute value of speed is less than friction, set speed to 0
With forward and reverse done, we want to do it for left and right
For a car, if you just increment or decrement x by a value, you will get the left and right movement for a car in a browser game
To get a realistic simulation though, that is not ideal as it does not comply with the laws of physics
Instead, we will add a new property in Car called angle and set it to 0
For left controls, set angle to an increment of 0.03 for now
For right controls, set angle to a decrement of 0.03
The idea above is to allow the car to rotate at an angle when turning left or right
To introduce rotation, in your draw method, wrap the inside with a save and restore method so the rotation only applies to the car
Inside the save and restore method, call translate on ctx, passing in x, and y of the Car object, and call rotate at negative angle on ctx
Remove the x and y coordinate component in your rect, leaving the negative half of width and height intact because we are already translating it using translate on ctx
Now, pressing left or right will rotate your car
However, your car will move forward and reverse along the canvas regardless of where it is facing which is not what we want
To fix this, in your update method, set x to a decrement of Math.sin of angle multiplied by speed
Also, remove your previous y assignment, and set y to a decremtn of Math.cos of angle multiplied by speed
Playing around in your browser you will notice that when you turn left or right, it follows the top left or right edge of your car, even in reverse, which is not what we want
Also, we do not really want the car to be able to rotate in place(because car yada yada)
To fix it, in update, we set a condition when speed is not equal to 0, set flip variable to a ternary operator where if speed is more than 0, set it to 1, and if not, set it to negative 1. Inside your conditions for left and right, multiply the increment and decrement by flip(this will multiply the value of angle by negative 1 when speed is not more than 0, and when it is not the value wouldn't be affected)
Now your car should move like car(sorta), and it cannot rotate in place anymore
Comment/remove your debug consoles once your controls are what you want
We can refactor the code for movement inside the update method by moving all the code inside Car update into a PRIVATE move custom method, and just call PRIVATE move method inside update
