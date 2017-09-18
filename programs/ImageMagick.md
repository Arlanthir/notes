# ImageMagick

## Resize
```
convert image.png -resize 64x128\! resized_image.png
```
`!` ignores aspect ratio, must be escaped with `\` in Linux.  
If omitted, the dimensions are treated as the maximum dimensions and the ratio is maintained.

## Batch Resize
```
mogrify -resize 64x128\! *.png
```

## Crop
```bash
# <FinalWidth>x<FinalHeight>+<originX>+<originY>
convert image.png -crop 444x406+20+10 image_smaller.png
```

## Extend canvas
```
convert image.png -gravity center -background none -extent 392x392 image_larger.png
convert image.png -gravity northwest -background #ffffff -extent 399x375 image_larger.png
```

## Append images
```bash
convert img1.png img2.png -append output.png     # Vertical
convert img1.png img2.png +append output.png     # Horizontal
```

## Save as 8bit PNG
Useful after using tinypng / pngquant to convert your original images  
Type savefile as `png8:output.png` instead of `output.png`

## Change animated GIF loops
```bash
convert animation.gif -loop 0 animation0.gif    # Loop forever
convert animation.gif -loop 1 animation1.gif    # Run only once
```
