# TouchDesigner 2d normals example

![image](https://user-images.githubusercontent.com/59149612/184504175-e7cead47-a4ed-49ef-8eb8-c73ba74bc712.png)

## Background
While working on a project, I wanted to generate outward pointing normals for some geometry for the sake of producing particles. Searching for information about this kind of thing, I came across this post where someone else was looking to do the same thing: https://forum.derivative.ca/t/changing-point-normals/12083

Reading the responses there, I realized:
- I could use the Point SOP to generate the data I wanted
- I had a simple 2D case here, and the normal I wanted could be the 2D normal to the segment between the next and previous point
- The normal to the segment dx/dy is -dy/dx

Given these facts, I wrote some expressions in a Point SOP and everything got terribly slow. That's when I remembered that I'd watched some tutorial recently in which someone had used a Slope TOP to basically do the same thing, so I went SOP -> CHOP -> TOP, used the Slope TOP and some other things, and reversed the pipeline. It generally worked!

Then I realized that there was a Slope CHOP and that the TOP part of things was unnecessary, which left me with the solution you see here.

I did notice at the end of things that I had an outline, not the solid primitive that I had to start with. I'm no SOP expert, so I tinkered with options until I found something that worked (Join SOP), but I have no idea whether it's the best option.

## Network description
In case you like text descriptions of Touch networks 🤷:
- convert to CHOP, keeping position and uv
- Select CHOP the tx, ty, and tz
- use the Slope CHOP to get the slope for adjacent points
- run through a Math CHOP that multiplies by -1 with the scope set to only one of the two components
- Rename CHOP to both reverse the x and y and rename from positional to normal (tx, ty, tz => ny, nx, nz)
- Merge back in with position and uv data from the initial CHOP conversion
- Convert back to SOP
- Run through Join SOP to create closed primitive
