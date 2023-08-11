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
Now, create a new js file called sensor.js and link it in html
In sensor js, create class Sensor, passing in car as reference(sensor will be attached to the car)
Convert car to class property in Sensor
Set rayCount to 3
Set rayLength to 100
Set raySpread to Math.PI divided by 4(45 degrees) --> Keep in mind Math.PI times 2 is a full circle
Set rays to an empty array
Create custom update method for Sensor
Inside Sensor update, set rays to empty array again
Then, create a for loop with a limit of i less than rayCount
Inside the loop, create rayAngle variable and assign it custom utils function lerp(linear interpolation), passing in raySpread divided by 2, negative raySpread divided by 2, and i divided by rayCount minus 1(because your limit is less than)
Then, inside the same for loop, set start to an object with coordinate x pointing to car's x, and y pointing to car's y
Set end to to an object with coordinate x pointing to car's x minus Math.sin of rayAngle multiplied by rayLength
And coordinate y to y pointing to car's y minus Math.cos of rayAngle multiplied by rayLength
After defining the above, use push method on rays array, passing in an array of start and end as array items(just like you did for borders array)
Create a custom draw method, passing in ctx and create a for loop with a limit of i less than rayCount
Inside that loop, you will draw these raylines by calling beginPath on ctx first as always
Define lineWidth of ctx to 2 and a strokeStyle of yellow
Then, call moveTo on ctx with coordinates of x's rays array at the 0 index of i index(oof), and y's rays array at the 0 index of i index(0 index is start)
Then, call lineTo(where the end coordinate of the line should be) on ctx with similar coordinates BUT at 1 index of i index(end variable)
Once you have defined moveTo and lineTo with their coordinates, stroke ctx to draw them
With the above done, we will need to bring the sensor class to your car, so set sensor in Car with an instance of Sensor, passing in the car reference(since you are already inside Car you can just pass in this keyword)
Then, in Car update, call sensor's update
In Car draw, call sensor's draw, passing in its reference
After all that, you should see 3 yellow lines protruding from your car object on your browser(cuz rayCount is 3)
Currently, the sensors are static (they face the same direction regardless of how your car moves)
We want the sensors to move according to where the front of the car faces
To do that, in Sensor update's rayAngle variable, add the car's angle to lerp and it should work
You can increase rayCount value and it will simulate a car's front flashlights if you increase it to a high enough value
If you change rayCount to 1, there is no ray because your lerp function has rayCount - 1 in its t parameter which will result to 0
To fix this, you can replace that argument to a ternary operator where if rayCount is exactly 1, set it to 0.5 instead(centerpoint of the ray spread), and if not, set it to the same value as previously(i / this.rayCount - 1) --> This is just to solve this edge case
As usual, we want each method to do one job only (one principle methology), so each custom method should be PRIVATE here
Cut the code in Sensor update and move it to PRIVATE castRays method, then call it in the update
We want our sensors to be able to detect the borders
To do that, we need to pass in the road's borders array into car update in animate
Then, pass roadBorders reference to car update, and the update call on sensor inside of it
Back to sensor js, pass roadBorders reference to its update method as well
As such, roadBorders reference will be linked from the rays to the borders array(though it does nothing as of now)
In Sensor, add readings property and assign it an empty array
In Sensor update, initialize readings to an empty array
Then, add a for loop with a limit of i less than rays array length, calling a push method on readings array, pushing in a PRIVATE getReading method(not yet made) with a i index rays, and roadBorders reference
With the above done, we can create a PRIVATE custom getReading inside Sensor to introduce the logic between rays and borders interaction
Pass in ray, and roadBorders reference to PRIVATE getReading
The rays will detect when it touches objects you deem to be obstacles, such as borders or other cars(which will be added later)
The rays will find the coordinates of all the obstacles they touch, and they will only keep the points of intersection that are closest to the point of origin(in this case, the car you are 'driving') --> this is how sensors work
In PRIVATE getReading, set touches variable to an empty array
Then, set a for loop with a limit of i less than roadBorders length, and inside the loop, set touch to a custom getIntersection method(not yet made), passing in ray at index 0, ray at index 1, roadBorders at index i and 0, roadBorders at index i and 1
After the above, and still inside the for loop, set a condition where if touch is true, push touch to touches array
After the for loop, if there are absolutely no touches(touches array length is 0), return null
Else, set offets to a map call on touches, and for each element, get its offset(map method goes through all available offsets in touch variable and return each as an array)
\*\* Note that the arguments in getIntersection are essentially coordinates(x, y, offsets)
Out of all these arrays being returned using map, we only want the closest/minimum value
So, we set minOffset to a Math.min of the spread of offsets(since map returns each offset as an array)
Once we get them, we return the touches array, calling find on it, and for each element, only find the offset that is exactly minOffset
With the logic above done, we will want to draw the readings out
Before that you will need the getIntersection method fleshed out(because author didn't really include it)
getIntersection method, just like lerp, will be in your utils.js, as the logic will be required(It's called segment intersection and you can get more in-dept explanations on his channel(probably))
All the offsets thingy at the front refers to segment intersection
\*\*The basic explanation of segment intersection is to use linear interpolation of two intersecting lines and find their points of intersection
Now, back to Sensor draw, add end variable to the start of the for loop, and set it to rays array at i index and 1 index
Then, if readings at i index is true, set end to readings at i index
In the lineTo call on ctx in Sensor draw, replace the rays array at i index and 1 index to end variable(since you have defined end variable as such)
After the above, duplicate the yellow line code and change it to black for the second set
Then, instead of drawing at the start of the rays like the yellow lines, draw it at the end of the rays (rays[i][1]) instead
The above, along with the getIntersection method, will make it so that when your rays touch the borders(and later when other cars are added), the part where the rays touch them will turn black
Currently, we are drawing the car using simple line draws, and in doing so, we are unable to deterministically detect the coordinates of the car's corners/anglepoints, which will be required to detect collisions(i.e when a car crashes on obstacles)
As such, we will need a custom method to find out the car's corners' coordinates on the canvas
To do this, we will have a new PRIVATE createPolygon method and in it, we will write the logic on how to detect where the corners of the car are(this method can also be used for other shapes, hence the Polygon name)
Go to car.js and create a PRIVATE createPolygon method, right below car update(this method will be called in update)
Inside createPolygon, initialize points to an empty array(this will contain where all points of the car/shape are at)
For a rectangle, like the current car, the coordinates of the corners by going from the mid point of the rectangle, and drawing a line towards the corners, forming a triangle
This line's value can be found using the hypotenuse method of the width and height divided by 2
The angle of the triangle will require the use of the arc tangent method(which is the width divided by height) --> You can use Math.atan2 and pass in width and height
Once you have the calculations above stored in 2 different variables(in this case, I use hypo for the Math.hypot calc, and hypoAngle for the the angle), use push method on points array, passing in an object with x,y properties
x will be x minus Math.sin of angle minus hypoAngle, then multiply by hypo
y will be y minus Math.cos of angle minus hypoAngle, then multiply by hypo
The above push is the calculation for the top right corner of the car
We will do it for top left now, with the same calculations, but with plus hypoAngle for both x,y instead
For bottom right and left corners, we will need to add Math.PI to their Math.sin/Math.cos calculations, to add a 180 degree spin(basically the top right and left calculations but in reverse)
Then, return points array
With createPolygon done, in car update, set polygon to a createPolygon call after we move the car(so after #move call)
Now, we can use createPolygon to draw the car instead, so in car draw, you can remove your existing save and restore method and all the code between it where we use multiple methods to manually draw the car out
To draw the polygon, call beginPath on ctx like before
Then, call moveTo on ctx at index 0 of polygon's x, and index 0 of polygon's y coordinates
After that, create a for loop with i at 1(because moveTo is already index 0), and a limit of i less than polygon's legnth
Inside the for loop, call lineTo on ctx and pass in i index of polygon of x and of y(this will loop through all available data in the points array stored inside polygon variable)
Then, call fill on ctx to draw the car which is 'polygonified'
Although there is visually no change, you can now go to your createPolygon method and tweak the values inside the calculations of your x,y coordinates to change the shape of your car object
With the car drawn out using math, we can now calculate when a collision happens between the car and obstacles using math as well
In Car, set damaged property to false
In car update, after polygon, set damaged to a PRIVATE assessDamage call(not yet made), and pass in roadBorders
Now, create PRIVATE assessDamage method, passing in roadBorders reference
Inside the above method, create a for loop, with a limit of i less than roadBorders length
Inside the for loop, if polysIntersect method(not yet made) with polygon, and i index of roadBorders references is true, return true
After the for loop return false -> if the code reaches here, that means there is no intersection between polygons
You will now create a polysIntersect custom method in utils js, using segment intersection logic
In your utils js, create polysIntersect method and pass in poly1 and poly2 references
Then, create a for loop, with a limit of i less than poly1 length
Inside that for loop, create a second for loop with a limit of j less than poly2 length
\*\*\ The above is a javscript method of comparing between 2 values/points simultaneously as the loop runs
Inside that second for loop, create touch variable, and assign the getIntersection method to it
You will need 4 arguments for getIntersection which are i index of poly1, i + 1 index of poly1 MODULAR of poly1 length, j index of poly2, j + 1 index of poly2 length
\*\* Imagine a rectangle with 4 points p0 to p3, p0 will connect to p1, p1 to p2, p2 to p3, but p3 to p0 will not connect because this is a for loop with a limit of poly1 length. As such the i + 1 will guarantee that connection from p3 to p4 but the modular operator will change p4 to p0 so p3 will connect to p0 --> this is how i understand it, could be wrong
The touch variable is used in this function to take all segments of the first polygon and comparing against all segments of the second polygon
After touch variable, if touch is true, return true --> meaning there is an intersection of polygons
After the first for loop, return false --> meaning there is no intersection if code reaches here
To test the code, back to car js, in car draw, set a condition if damaged is true, call fillStyle on ctx to gray
Else, call fillStyle on ctx to black
Whenever your car touches the border, it should change color to gray to indicate that it is 'damaged' and that the polysIntersection function is working with car as poly1 and border a poly2
\*\* Note that when your car is slightly touching the border, it will not turn gray and this is because the line itself is very thin that is calculated by math and has no thickness, but the border we drew is manually drawn using javascript and we added lineWidth to it so the 'real' intersection does not happen until the car truly intersect pass the border which visually makes more sense(probably)
In car update, set a condition if damaged is false(!damaged), move the code from #move till damaged inside the condition --> so if car is not damaged, allow it to move and such
Now, your car should stop moving when it turns gray(gets damaged)
We will now create other obstacles, in this case, other cars(to simulate traffic)
In main js, initialize traffic and give it an array with a new instance of Car at a different y coordinate(so it will be above your original car)
Then in animate, create a for loop with a limit of i less than traffic length and call update on i index traffic, passing in the required reference(place it before update call on car)
Also, create another for loop with a limit of i less than traffic length before car draw, and call draw inside the loop on i index traffic
Doing the above will make another car appear above your car but notice that you are now controlling the above car instead of the original
This is because the key listeners for the car is being overwritten by the last car available in the i index traffic
To fix this, we will need to specify which car should have controls enabled and which should not
In main js car variable, pass in an additional parameter "KEYS"
For the Car in traffic array, pass in "DUMMY"
In Car js, pass in a new reference called controlType(to label the car KEYS or DUMMY)
Then, pass controlType into the new instance of Controls in controls property
In controls js, pass in type in the constructor
Inside Controls, add a switch statement with type reference
Inside the statement, for case "KEYS", call PRIVATE addKeyboardListeners on controls(this), break, then for case "DUMMY", just set forward to true
Remove the previous PRIVATE addKeyBoardListeners(because it is now inside the switch statement)
Now, you should see the DUMMY car move forward at maxSpeed as defined in your Car class, making it impossible for your car to ever catch up(because your car and the dummy car has the same speed, making you unable to test collision between cars)
To vary the speeds, you can add maxSpeed as reference in your Car constructor and set it to 3, and convert maxSpeed to class property
Back to main js, you can change maxSpeed reference value to let's say 2, which means your controlled car will have default 3 maxSpeed, making it possible to catch up to the dummy car(you can also set maxSpeed in your main js now in your Car instances)
Currently, the sensors is unable to detect other cars and unable to be damaged by it, and also we do not want sensors on dummy cars too
To disable sensors on dummy cars, in car js, add a condition where if controlType is not equal to DUMMY, then we instantiate a new instance of Sensor assigned to sensor property
In Car update, you will also need to add a condition where if sensor is true, then call update on sensor
And also the same condition in car draw, then call draw on sensor
Your dummy car should be devoid of sensors now
To allow the controlled car to collide with dummy car, pass traffic array to car update in animate
Also, pass in an empty array to the for loop on traffic update instead of traffic (this is to prevent dummy car from colliding on itself if traffic array was passed instead, and in the future, will prevent dummy cars from colliding on each other and create road blockages --> although this will make it cooler but a hassle as you only want to test sensors on your self-driving car)
In car js, you will need to pass traffic to car update as well
Pass traffic in damaged in car update, sensor update in car update, PRIVATE assessDamage method
Inside assessDamage, add a second for loop like your roadBorders for loop, but replace roadBorders limit with traffic instead
Inside the polysIntersect method for traffic loop, replace i index roadBorders to i index traffic's polygon instead
Now, when your controlled car collides on dummy car, it should turn gray, indicating it is damaged
To let your sensors detect oncoming dummy cars, go to sensor js
In sensor js, pass in traffic array to sensor update
Inside the for loop in sensor update, in PRIVATE getReading method in the push method, pass in traffic reference as well
Also pass it in the custom PRIVATE getReading method too
Similar to the for loop for roadBorders in getReading, we will now have a second set of for loop for traffic using the same methology
Create that second for loop with traffic instead, and assign poly to i index traffic's polygon(this is to prevent yourself from writing traffic[i].polygon everywhere)
Then, inside that traffic loop, create a secondary for loop for j index(because we are now calculating collision between two polygons so we are using the extended segment intersection in utils js methodology), and call getIntersection method, assigned to value, passing in 0 index ray, 1 index ray, j index poly, and \*j + 1 MODULAR poly.length index poly --> Remember this? if not go back to utils to check it out
Then, if value is true, push value into touches array
The sensors should turn black when touching the dummy cars but since sensor turns black and the car is black it is hard to discern, so you should color the car differently
Go to main js, pass in "red" in traffic draw in animate
Then, pass "blue" in car draw in animate
After the above, go to car js and pass in color reference to draw method, and inside the condition for non-damaged car fillStyle, set it to color reference
Now, you should be able to see the sensor lines intersecting with the dummy cars more reliably
We will now implement one of the main points of this project, neural network, which will allow the car to detection oncoming collisions, and use the key listeners from controls to move out of the way
Create new js file call network.js
In network js, create Level class, passing in inputCount and outputCount references
Convert inputCount to an instance of new Array assigned to inputs
Do the same for outputCount for outputs
Create biases property(a value above output which will fire off when it reaches that value), and assign a new instance of Array of outputCount to it
Create weights property and give it an empty array
Each input neuron will be connected to every output neuron
So, after weights, create a for loop with a limit of i less than inputCount, and inside, assign i index of weights array to an instance of new Array of outputcount
The above for loop is just a 'shell' that connects each input to each output and we will need to put actual values for them to actually function
We will begin with a randomize version by calling a PRIVATE randomize method(not yet made) on Level in Level class properties, passing in the this keyword
Inside Level, create a static PRIVATE randomize method, passing in level --> use static because author wants to \*serialize(no idea what it means yet) this object afterwards
We will compare between level reference inputs length, and its outputs length using for loops and for each i and j index of weights array in level, we are assigning a randomized value between -1 and 1 (the negative value is there because there are 2 possible directions to turn when avoiding a frontal collision, left and right, and the negative value on for example, right, will 'warn' the car that right will cause collision as well, which will prompt the car to turn left )
After the above, create another for loop for biases and randomizing the level reference i index biases between -1 and 1 as well
We will now create a static feedForward method, passing in givenInputs and level references
Inside it create a for loop with limit i less than level's inputs length, and assign each i index of inputs in level to i index of givenInputs
Then, create another for loop with limit i less than level's outputs length
Inside the second for loop, create sum variable assigned to 0(holder variable), and we are going to calculate the value of the inputs and the weights, which will be calculated and placed inside sum variable
After initializing sum, create a for loop for j index, with limit of j less than level's inputs length
Inside it, we are going to increment sum by j index inputs in level multiplied by j and i index weights in level
After the above loop, check if sum is more than i index of biases in level, and if so, set i index of outputs in level to 1(on), if not, set it to 0(off)
Then, return outputs of level
The above code for randomize and feedForward represents the first level of the neural network
\*\* The function used above is called a hyperplane equation, which is extrapolated from the basic line equation
\*\* Line equation is weight(w) times sensor input(s) + bias(b) equals 0 (ws + b = 0). The weight controls the slope, while the bias controls the y axis, which as you know from above bounces between -1 to 1
\*\* The plane equation, uses 2 sensors so it will be w0s0 + w1s1 + b = 0, representing 3d space
\*\* The hyperplane equation, which we used, will include more than 3 sensors or more (w0s0 + w1s1 + w2s2 + wnsn... + b = 0), because with more than 3 sensors it is harder to visualize due to higher dimensions, but the math works all the same
Back to network js, above Level class, define another class called NeuralNetwork, and pass in neuronCounts reference
Inside NeuralNetwork, set levels property to an empty array which will be filled with levels defined in Level class(level 0, 1, 2 etc)
To do the above, we will create a for loop, with a limit of i less than neuronCounts length minus 1 (\*this will be added back inside the for loop for each i index because we are only using the number 0 and 1)
Inside the for loop, use push on levels array, pushing in an instance of new Level, passing in the required inputCount and outputCount references
inputCount will be i index of neuronCounts, while outputCount will be i + 1 index of neuronCounts (\*number added back here)
We will also need a static feedForward method for NeuralNetwork as well
So, after the for loop, create static feedForward in NeuralNetwork, passing in givenInputs and network references
Inside this new feedForward, get/set outputs to feedForward call on Level class, passing in givenInputs as reference, and index 0 levels array on network(first level)
Then, we will use a for loop to loop through the remaining levels
So, create a for loop with i assigned to 1 (because the first level is already created), and a limit of i less than network's levels' array length
Inside the above for loop, each loop should set outputs to feedForward's call on Level, passing outputs, and i index of levels array of network
Then, return outputs
\*\* The above feedForward function is essentially putting in the output of the previous level, into the new level as the input. And the final outputs will tell us if the car should go which direction(wsad)
Now, we want to connect/link the code inside network.js to our car object
Inside car js, in the property where we defined sensor, set brain to an instance of new NeuralNetwork
For the neuronCounts reference, we want to specify our array of neuronCounts, so pass in an array with sensor's rayCount, a hidden layer value of 6, and output layer of 4(signifying the 4 directions)
In car update, after sensor update, set offsets to sensor's readings, and call map on readings, and for each sensor(or s), if it's null, return 0, else, return 1 minus s.offset
\*\* The author is doing the above he wants neurons to receive low values if object is far away, and higher values close to 1 if object is close, kinda like a flashlight where the return light values are weak when pointing at a far object, and stronger return light values when the flashlight goes closer to the object. This is also how sensors work in practice
Now, set/get outputs after the map method above, but calling feedForward on NeuralNetwork, passing in offsets variable, and brain property
Console log outputs for now to check some values
Currently, the car is not going forward because the 'brain' is not connected to the controls
To allow the car to be 'self-controlled', back in main js, instead of "KEYS" in car, replace it with "AI"
Then, in car js, define useBrain in Car property and assign it to a controlType where "AI" is true
In car update, inside the sensor condition, set a condition where if useBrain is true, set controls' forward to index 0 outputs, controls' left to index 1 outputs, controls' right to index 2 outputs, and left, and reverse to their appropriate index, according to your Control constructor properties
Now, your car should have an array of 4, each representing a direction, 0 meaning it's not pressed down, and 1 meaning it's active
Refreshing your browser will randomize your controls, as defined by the PRIVATE randomized in Level, making your car go according to the directions being pressed
The console log above do not really paint your picture and your car behaviour is still very random because we do not know what is happening
Remove the console log and inside browser console, type car.brain to take a look at the properties of each iteration when refresh to get a better picture(but still confusing)
To solve this 'confusion', it would be better if the network is present alongside the car, so we can better understand how each value is affecting the movement
We will refactor canvas into two separate canvases, carCanvas and networkCanvas
Go to your css file and do the necessary changes, applying contrasting background colors to both canvases
Rename all canvas to carCanvas in main js, and ctx to carCtx
Below carCanvas, create networkCanvas variable with the same assignment as carCanvas, but with networkCanvas, and change width to a value larger than carCanvas'
Below carCtx, create networkCtx(same as carCtx)
In animate, below carCanvas height, do the same for networkCanvas
In your browser, you should see a separate canvas for your network with your appointed color
Copy and paste the code for Visualizer into your utils or main js and call drawNetwork on Visualizer, passing in the required references of networkCtx, and car.brain(\* if you wanna know more, go to the author's visualise network video)
To visualize it even better, pass in time in animate(which is an in-built frame per second from requestAnimationFrame, called deltaTime in your other projects), and call lineDashOffset on networkCtx, assigning it negative time divided by a value to slow down the animation(placing only time is too fast as it is like 60 frame per second)
Doing the above would show you visually the direction the feedForward functions are heading
Now, we will generate multiple cars in parallel to test out the network(because testing it with 1 car is tedious)
Above animate in main js, create custom method generateCars, passing N reference
Assign cars variable to an empty array inside
Then, create a for loop starting at i index of 1, and a limit of i less than or equal to N
Inside the for loop, push an instance of new Car into cars array, passing in the same references as your original car
Return cars array
For your car variable in main js, change it to cars and assign it the custom method generateCars, with the N reference
Then, set N variable before cars and set it to 100
The idea here is that 100 cars is going to run simultaneously, and 1 of them is going to behave the way we want it to behave
In animate, you will need to change car update and car draw since you are using cars array now
For car update, replace it with a for loop, with a limit of cars array length, and inside it, call update on i index of cars array
For car draw, also replace it with a for loop, with the same limit, and call draw on i index of cars array inside it
The car variable inside translate and the reference for drawNetwork in animate will need to change as well
We only need 1 car so just replace car with cars and 0 index for both, keeping the rest the same
Checking your browser, you should have a 100 cars simultaneously going off, but you cannot really make anything out of it
You can change the transparency of the 'parallel' cars with globalAlpha to see things better
Before the for loop for cars draw, set globalAlpha of carCtx to 0.2, then set globalAlpha back to 1 after the for loop
We want to emphasize 1 car, so let's take 0 index of cars for now and call draw on it after setting globalAlpha to 1
Pass in the usual carCtx, "blue", with an additional true argument
The additional argument is so that only this car will have sensor out of all the cars
So, go to car js, and add an additional parameter in draw called drawSensor = false
Inside car draw, add an AND operator in the condition for sensor, and add in drawSensor as a condition as well
Inspecting your browser, you should notice that the car that has the sensors is not really the car that you want to look at as it is just one of the 100 random cars that you have put inside the canvas
The one you want to follow is the car that is actively pursuing the other car while not crashing into it, and bypass it smoothly
We will call this car the bestCar
Inside main js animate method, after cars update, set bestCar and use find method on cars array, and for each car, find the car.y that has the minimum value(Math.min), of a spread of map of each car's y value in cars array --> \*\ The reason for doing this is because Math min does not work with arrays, only with numbers, and map method gives you an array of y values of each car, and spread will turn all these arrays into values
With bestCar initialized, we will use it to replace all index 0 cars array(in translate, draw on index 0 cars, and index 0 cars on brain in drawNetwork)
We will need to refresh multiple times to get a car like that(100 is not really much) so we should implement a save method on localStorage to store the bestCar that meets our expectation, and a discard method, if we need to discard it later down the road(pun not intended)
Inside main js, before generateCars, set save method, and in it call setItem on localStorage, passing in "bestBrain", and a JSON.stringify on bestCar's brain
Also, create a discard method, and call removeItem on localStorage on "bestBrain"
These methods will not be able to 'read' what bestCar is since its a local scope inside animate, so you would have to set it at a global scope instead
Set bestCar globally and assign it index 0 of cars array
Then, set a condition where if getItem call on localStorage of "bestBrain" is true, set bestCar's brain to a JSON.parse of getItem call on localStorage of "bestBrain" (JSON parse is needed because localStorage only works with strings)
Now we will like these save and discard methods to be used at a click of a button so we will need to create buttons in html
Set a div between the canvases and give it the id verticalButtons
Inside the div, set 2 buttons with onclick save() and onclick discard()
In CSS, we will now use flex display to move the icons
In body, remove align text and set display to flex
Then, justify content to center(horizontal), and align items to center(vertical)
Add id verticalButtons to css and also set its display to flex
Set its flex direction to column
For button tag, set its border to none
A border radius of 5px, padding of 5 5 7 5, margin of 2, and a pointer cursor
Give the button:hover a background of blue
With the above done, go to your browser and refresh till you get the car that sidestep the obstacle car and not crash onto the border, and you can click on the save icon to save it into localStorage
Currently we are only using the y coordinate to determine which car we are tracking(bestCar), and for a non-linear road this would not be ideal
The code inside bestCar is known as a fitness function, and its purpose is to try and find a function that best fit your objective(finding the ideal car to avoid collision), while also figuring out the edge cases and how to mitigate them(i.e if using distance as a metric a car going in circles will be an edge case)
For now, let's add more traffic in lane 1(0), and lane 2
Go to traffic array in main js and set two instances of DUMMY new Car at lane 0 and 2, with y coordinates of -300(further up) for both of them
After trial and error-ing in your browser, the 'shaky' car is the car that is close to your ideal car but not quite because it cannot go past the second set of dummy cars
Also, there is only one 'shaky' car at any given iteration and we would like more of these 'shaky' cars to increase a chances of one of them being able to pass the second set of dummy cars
To achieve the above, we will now mutate the network
In your NeuralNetwork class, create a static mutate method, passing in a network, and an amount assigned to 1 as parameters
Inside the mutate method, we will first use a forEach method on levels array of network, and for each level, we are going to iterate(for loop), with a limit of i less than biases array of each level
Inside the for loop, for i index biases of level, we are going to linear interpolate(lerp) between the current value of the bias(i index of biases of level), and a random value between -1 and 1, depending on amount(A + (B - A) times t)
After the above iteration, we will iterate through the weights of each level(both i and j indexes), and for those i and j weights on each level, linear interpolate between the current value of the weights, and random value between -1 and 1, depending on amount
The for loops above work similarly like your randomizer code but the mutate method will apply to the entire network due to the for loops
How mutate works is if the amount parameter is 0, then everything stays the same, but if it is between -1 and 1, then it will move away from the biases depending on the value of the amount(this is where the mutation happens)
Back in main js, for your condition of bestBrain, create a for loop inside with a limit of i less than cars length and move the JSON.parse code inside it but bestCar.brain will be cars[i].brain now
If you test it out now, then all cars will be just that one 'shaky' car that sidesteps the first dummy car and crashes on the second set
We will need a condition for all the other cars to not follow the current bestCar
So, in the for loop above, set a condition where if i is not 0(not the current bestCar, which means all the other cars), we will call mutate on NeuralNetwork, and pass in the brain of i index cars , and an amount of 0.1 for now
Once your bestCar passes the second set, we will need more traffic to test the car even more
Back in traffic array, add like 4 more dummy cars at even further up on the y axis at the different lanes to complicate the road to test your car
You can change N to a higher value so that you can iterate through more cars
Remember to save the car that performed the best
