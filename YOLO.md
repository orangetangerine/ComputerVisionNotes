## Anchor Boxes
What about in the case of overlap, in which one grid cell actually contains the center points of two different objects?

We can use something called anchor boxes to allow one grid cell to detect multiple objects.

In this image, we see that we have a person and a car overlapping in the image. So, part of the car is obscured. We can also see that the centers of both bounding boxes, the car and the pedestrian fall in the same grid cell. 

Since the output vector of each grid cell can only have one class, then it will be forced to pick eigher the car or the person.

But by defining anchor boxes, we can create a longer grid cell vector and associate multiple classes with each grid cell.

So the outout vector will now contain `16` elements(8 for each object box)

But it can only associate one object with each type of anchor box, so it doesn't work very well if you have two overlapping objects that are roughly the same shape. Similarly, if you only define two anchor boxes but have three overlapping objects, then this algorithm fails because it will be forced to identify only two of the objects. The good news is that the above cases are womewhat rare.

### How YOLO works
Once the CNN has been trained, we can now detect objects in images by feeding at new test images.

The test image is first broken up into a grid and the network then produces output vectors, one for each grid cell. These vectors tell us if a cell has an object in it, what class the object is, and the bounding boxes for the object. If we uses two anchor boxes, we'll get two predicted anchor boxes for each grid cell. Some, in fact most of the predicted anchor boxes will have a very low Pc value.

After producing these output vectors, we use non-maximal suppression to get rid of unlikely bounding boxes. For each class, non-maximal suppression gets rid of the bounding boxes that have a Pc value lower than some given `threshold`. It then selects the bounding boxes with the highest Pc value, and removes bounding boxes that are too similar to this. It will repeat this until all of the non-maximal bounding boxes had been removed for every class.