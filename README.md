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
Inside the above method, create a for loop, with a limit of i less than roadBorders length, and if po
