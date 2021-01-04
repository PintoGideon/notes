SVG isn't just an image. SVG is a document.

By default, the SVG will be drawn at the size specified in the code.

### The SVG Scaling toolbox

The viewbox attribute is whole lot of magic rolled up in one little attribute.

The viewbox defines the aspect ratio of the image.
It defines how all the lengths and coordinates used inside the SVG should be scaled to fit the total space
available.

The viewBox is an attribute of the <svg> element. Its value is a list of four numbers, separated by whitespace or commas: x, y, width, height. The width is the width in user coordinates/px units, within the SVG code, that should be scaled to fill the width of the area into which you’re drawing your SVG (the viewport in SVG lingo). Likewise, the height is the number of px/coordinates that should be scaled to fill the available height. Even if your SVG code uses other units, such as inches or centimeters, these will also be scaled to match the overall scale created by the viewBox.

Just like viewBox, ```preserveAspectRatio``` has a lot of information in a single attribute. The default behavior can be explicitly set with preserveAspectRatio="xMidYMid meet". 

The first part, xMidYMid tells the browser to center the scaled viewBox region within the available viewport region, in both the x and y directions. You can replace Mid with Min or Max to align the graphic flush against one side or the other. Watch the camelCase capitalization, though: SVG is XML, and is therefore case sensitive. The x is lowercase but the Y is capital.

