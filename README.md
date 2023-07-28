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
Now for the road, create road.js, and link it to html
In road.js, create Road class, passing in x to center road on x, a width for a set width for the road, and laneCount assigned to the value of 3
Convert them to class properties
Set left and right in Road to be at x and half of width(Remember going left is negative while going right is positive when along the x axis in a computer)
For y axis, you want it to be infinite, so set infinity to a very large number(i.e a million), and set top and bottom to infinity(Again, upwards of y is negative, while downwards is positive)
Create custom draw method for Road, passing in ctx
Inside draw method, set lineWidth of ctx to 5, and strokeStyle to white
Begin drawing the line by calling beginPath on ctx, and call moveTo from left, top coordinate
Then, call lineTo from left to bottom, and call stroke
The above drawing will draw a vertical line on the left side of the canvas
Create another set and draw a vertical line on the right side of the canvas
Back to main js, above your car variable, create road variable, and set it to a new instance of Road, passing in the required references (x, width, and laneCount)
x will be half of canvas width, width will be canvas width(laneCount will be touched upon later)
Inside animate, call draw on road above car draw and you should be able to see the white line with 5px which you have defined previously
To create a margin on the sides of the road, you can shrink the width of the road by multiplying the width reference of road by let's say 0.9
For laneCount, back to Road draw, create a for loop, using less than or equal to laneCount as the limit
We will employ \*\*linear interpolation (THIS HAS NO IN-BUILT JAVASCRIPT SO YOU NEED A CUSTOM METHOD FOR IT)
\*\* Linear interpolation helps to find an unknown point between two coordinates(x1, y1, and x2, y2), because there is a constant slope between them and we can find an unknown x,y between the 2 sets of known x,y
In this case, we are linear interpolating between the left and right which we have defined, and between i and laneCount(0 and 3) and assign them all to x
In your code for left and right lines, you can replace left in moveTo and lineTo to x, and remove the code for right lines because the for loop will generate up to 3 lanes for us now
Move the line drawing inside the for loop to loop the code
\*\* Create the custom lerp function inside utils.js and link it to html
You should see 3 lanes now for your road
To create a more realistic road, we will create line dashes instead of just straight vertical lines in the middle
Inside the for loop, check if i is more than 0 AND i is less than laneCount, call setLineDash on ctx at 20,20 (20px line then 20px break loop for the middle lines)
Else, call setLineDash on ctx at an empty array(since the left border is at 0 and right border is at laneCount, they will remain straight lines)
You can change the value of your laneCount in Road to generate more lanes (but it will be limited by the canvas width)
For every lane, it will be useful to determine where the center of a lane in where we will be spawning cars in
To do that, add a custom method inside Road called getLaneCenter, passing in laneIndex
Inside getLaneCenter, set laneWidth variable to width divided by laneCount(this will help you get the full width of each lane)
Then, return left plus half of laneWidth(middle of each lane) multiplied by laneIndex times laneWidth(the position of each lane)
To test it, back in main js, in your car variable, instead of a hardcoded 100, pass in road's getLaneCenter with the laneIndex reference(0 to 2 if 3 lanes, 0 to 3 if 4 lanes etc)
Whatever value you put as laneIndex reference should spawn the car at that lane when you refresh
However, if let's say you reduce the number of lanes, to let's say 2 and pass in 3 in getLaneCenter reference, javascript will still spawn it at lane 4 outside the canvas(you can keep moving left using controls to make the car appear visually from outside the canvas)
To make sure that your car always spawns initially within the canvas, for the laneIndex times laneWidth portion, you can wrap laneIndex in a Math.min method, and pass in a second argument of laneCount minus 1 so that your car will always spawn at the rightmost lane of the canvas even if the value passed in your getLaneCenter reference is larger than the number of lanes
You can test it by passing let's say an overly large number such as 100 in your getLaneCenter in car variable, and your car should still spawn in the rightmost lane of your road even though you do not have 100 lanes
Currently, you can drive in and out of the canvas and we want to limit the car within the road
In Road, set borders property to an array
Inside the array, you will have an array with topLeft, bottomLeft array items, and array with topRight, bottomRight array items(the reason for doing this is for easy expansion of code for let's say if you want to add additional roads with different angles)
Above borders property, set topLeft to an object with x property pointing to left, and y pointing to the top
Do the same for topRight, bottomLeft, and bottomRight
Inside console, you can type road.borders to see the positions of your borders
Since you have added borders, to make your code more consistent, inside your for loop, change initial i to 1, the limit to laneCount -1, and remove the if condition, leaving only the setLineDash of 20,20 intact(Reason for doing this is we are refactoring the code to use the borders property now and this portion of the code will only draw lineDashes at each lane and the code for borders will be separated)
After changing the above, outside the for loop in Road draw, call setLineDash on context with an empty array at the end of draw, and use forEach method on borders array
In the forEach method, for each border, call beginPath on ctx, call moveTo from index 0 border of x, index 0 border of y coordinates, then call lineTo to index 1 border of x, index 1 border of y coordinates, and call stroke on ctx
The forEach method will be in charge of drawing borders of subsequent roads if you choose to add more roads
A way to have a 'camera' fixed on your car with a bird's eye view is to wrap road draw and car draw calls with save and restore on ctx in animate, then at the start of the save restore, call translate on ctx, passing in position 0,-car.y coordinate
You should see your car giving an illusion of moving because your road is scrolling while your 'camera' is fixed on the car
To make the car more visible, you can adjust it's y position in translate, by adding -car.y with canvas height and a value of 0 to 1(0.5 will be middle of canvas for example)
