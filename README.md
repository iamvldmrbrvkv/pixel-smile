# pixel-smile
[Codecademy](https://www.codecademy.com/learn) [TypeScript](https://www.typescriptlang.org/) project.

## Pixel Smile
It turns out we’ve been given a program that was half implemented by a colleague. It’s supposed to create pixel art outputting a smiling face to the console, but there are many errors and it doesn’t work.

In this project, we’re going to leverage TypeScript’s type system to help us fix bugs and guide our work implementing the missing functions. At the end, we’ll have a smiling face outputting to the console and the opportunity to draw our own pictures.
```js
XXXXXXXXXXXXXXXXXXXX
X                  X
X      X    X      X
X                  X
X   X          X   X
X   XXXXXXXXXXXX   X
X                  X
XXXXXXXXXXXXXXXXXXXX
```
## Tasks
### Understanding the Code
1. As we look over the code, we see:

- The global variables defining the image data.
- Some function calls to draw the head, eyes, and smile.
- A statement that calls the outputImage() function to output the information stored in imageData to the console.

These statements use the function declarations we see further down in the file—drawRectangle(), isPointInImage(), outputImage(), and finally createImageData(), which rely on the global variables declared at the top of the file.

2. Let’s first understand how the image data is stored.

Looking at the imageData variable, we see it’s initialized with the result of createImageData(). If we scroll down to where createImageData() is defined, we notice our colleague was kind enough to document what this function does and how the data is stored in the imageData array. Read over this documentation.

3. Attempt to compile the code according to the tsconfig.json file in the current directory by running the following in the console:
```js
tsc
```
What we can see from the output of the TypeScript compiler is there are many problems—compiler errors—with this code. The presence of these errors lets us know it’s probably not worth our time to try running the compiled JavaScript file yet as there’s a good chance it won’t work.

The compiler is letting us know about functions we’re missing:

- drawDot()
- drawHorizontalLine()
- drawVerticalLine()

There’s also an error on the outputImage() function call and errors inside the body of isPointInImage().

Take note of the number of compiler errors. We’re going to work on getting this number down to 0.

### Quick Wins
4. Let’s focus on work we can finish quickly. Look at the first error inside the isPointInImage() function—the third last error outputted by tsc.

TypeScript is pointing out that a boolean is not assignable to the type string. It could possibly be because the return type annotation for the function (: string) is wrong, or the return statement that returns a boolean is wrong.

In this case, we want it to return a boolean, so the return type is wrong. We could fix that by changing it to a boolean type, but let’s remove the return type annotation completely and let TypeScript figure out the function’s return type based on the return statement.

Make this change, save, and run tsc.

5. There’s still two other errors in the isPointInImage() function. TypeScript is complaining that y is possibly undefined, but we’re using it in a scenario where it should always have a number value. If we look up at the type annotation of isPointInImage() we can see that y is declared as an optional parameter which is causing this error.

Defining y as an optional parameter was a mistake because the caller should always provide a value for y. Change it to be required, save, and run tsc.

6. If we scroll down to the outputImage() function call, we see that TypeScript is telling us that it expects two arguments to be provided, but none were. The desired design here is to be able to call this function with no arguments, but TypeScript is raising an error because the second offChar parameter is currently marked as required.

This is a mistake in the declaration of outputImage(). To fix this, we’re going to provide a default value for the offChar parameter on outputImage() similar to what’s been done for the first parameter. Assign a space (" ") as the character to use for an “off” pixel by default, save your work, and run tsc.

7. You may have noticed that fixing the previous error caused a new one. TypeScript is now complaining about the offChar parameter’s use in the body of outputImage(). This error message is saying that the left-hand side of the expression offChar * 2 can’t be used in an arithmetic expression.

Now that TypeScript has the new information about what type our offChar parameter is, it knows it doesn’t make sense to use offChar—a string—in multiplication.

This was a mistake in the original code that TypeScript is now pointing out to us. It doesn’t make sense to have * 2 here because we only want to output the “off” pixel character. Let’s remove that multiplication, save, and run tsc.

### Drawing Dots
8. Our remaining errors have to do with functions that aren’t defined—they haven’t been implemented yet.

The drawDot() function might be best to implement first as we could use it in the other functions. Declare the function signature of the drawDot() function with parameters x and y. Both of these parameters should accept number types.

9. Within drawDot(), we need to set the imageData index for the specified x and y position to be true to designate that pixel as “on”.

There’s a formula documented above createImageData() on how we can set that.

10. To complete our drawDot() function, let’s ensure the caller can’t draw a dot outside the image bounds.

Write an if statement that only updates imageData if the provided x and y values are within the image by checking the result of the isPointInImage() function.

Save and run tsc.

11. After implementing drawDot(), some of our errors went away, but we got a new one on the drawDot(15, "4"); statement related to the string "4" being provided instead of the number 4.

This was a mistake. Fix the error by ensuring this function is provided a number, save, and run tsc.

### Drawing Lines
12. Continuing on squashing the errors, declare a drawHorizontalLine() function with x, y, and length parameters each typed as number.

13. Time to implement the function body of drawHorizontalLine().

Use a for loop to set each x position of the line at the same y position as being “on” by calling the drawDot() function for each pixel.

For example, you may loop i from 0 to the value of length incrementing each iteration by 1. Inside the body of the loop, call drawDot(x + i, y).

After doing this, save and run tsc to verify the number of errors is decreasing.

14. The last function we need to declare is drawVerticalLine() with x, y, and length parameters types along with its function body.

This should be implemented similarly to drawHorizontalLine(), but instead of increasing the x value for the provided length, we increase the y value.

### Compiling Without Errors and Running Our Code
15. Save the code and run tsc. It should run successfully now.

16. Congratulations, you’ve squashed all the errors! The TypeScript compiler has now outputted an index.js file in our workspace.

To run our code we need to execute the compiled index.js file with Node.

Run:
```js
node index.js
```
17. It works! Excellent job leveraging TypeScript’s type system to help you complete this task and have the code run seamlessly—when that happens it’s a special feeling.

You may want to challenge yourself to:

Use the functions to draw your own picture.
Add more ways of drawing to the image data (ex. drawCircle(x: number, y: number, radius: number), drawLine(x1: number, y1: number, x2: number, y2: number)).
Provide different values for onChar and offChar to outputImage (ex. outputImage("\u2588") or outputImage("O", "\u2588"))