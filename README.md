<div align="center">

## Advanced On the Fly button creator


</div>

### Description

This code will draw a button image based on what you specify for the image properties.
 
### More Info
 
You can configure the defaults in the script so you dont have to pass them in the query string but for query string use, they are. button height, width, font size, color, face, bg color, text vertical align, horizontal align.

Must have GD >= 1.6 installed with php. You can also download the zip file and some ttf fonts and get additional help and documentation from my website, http://www.drew-phillips.com

A png image based on user specifications.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Drew Phillips](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/drew-phillips.md)
**Level**          |Intermediate
**User Rating**    |4.7 (14 globes from 3 users)
**Compatibility**  |PHP 4\.0
**Category**       |[Graphics/ Sound](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/graphics-sound__8-15.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/drew-phillips-advanced-on-the-fly-button-creator__8-1080/archive/master.zip)

### API Declarations

Drew Phillips 2003


### Source Code

```
<?php
/*
You can also download the zip file and some ttf fonts and get additional help and documentation from my website, http://www.drew-phillips.com
Button Creator 1.0 by Drew Phillips - made July 19 2003
Copyright 2003 drew-phillips.com
REQUIREMENTS:
PHP >= 4.2.0 compiled with GD >= 1.6
Some TTF Fonts (included)
A basic knowledge of PHP
USAGE:
This is intended to be a quick way to generate buttons without
having to manually make images in a graphics program. It also is
useful for generating buttons on the fly for use in menus and links.
The general syntax for use is
img src=button.php?text=Desired%20Text&bg_color=black&font_color=blue&height=35&width=100
Here are some more examples to get familiar with:
img src=button.php?text=Home%20Page&font_face=archtura.ttf&font_color=purple
img src=button.php?text=Back&width=150&height=40&bg_color=red&font_color=blue
Here is the list of usable words in the query string:
text=text					- Text of button..This is necessary
height=y 					- Height of Image
width=x 					- Width of Image
bg_color=color				- Background color of Image
font_color=color				- Font color of text
font_face=face				- Font face of text
font_size=number				- Font size in pixels
align=left|center|right			- Horizontal text alignment
valign=top|middle|bottom		- Vertical text alignment
You may leave any of those parameters out, except text of course and the default
values will be used. The default values can be set down in the configuration
portion of this script.
If you have a default height of 35, width of 100, background color of gold and
text of black, a call of:
img src=button.php?text=Text
will make a 100x35 button, with gold background and black text, with 'Text' as text.
The usable colors are:
black, blue, crimson, cyan, gold, gray, green, indigo, lavender, lightblue,
lightgreen, lightgrey, magenta, navy, olive, orange, pink, purple, red, silver,
tan, violet, white, and yellow.
You may add your own colors to the list if you wish, but make sure you add them
in the following array form:
"colorname" => "012345678",
The color name is what color you want drawn and the 9 digits are the
rgb color values, the first three digits represent red from 000 (no red) to 255 (full red),
digits, 4-6 are green, and 7-9 are blue. If there are more or less than 9 digits it will
not work.
You may use hex values in the form of %0A to input values such as spaces, or newlines.
I wouldn't recommend using newlines because it will mess up the horizontal alignment.
Maybe in the next version I will fix that problem but right now I wanna get this out.
Hope this works well for you. Enjoy...
*/
##################################################################
##										 ##
## Configure defaults below						 ##
##										 ##
##################################################################
define("DEFAULT_IMG_WIDTH",100);
// This is the default width of the image, if none is specified in
// the query string. Enter an integer value, with no quotes around it
// Example: define("DEFAULT_IMG_WIDTH",350);
define("DEFAULT_IMG_HEIGHT",35);
// This is the default height of the image.
// Enter an integer with no quotes around it.
// Example: define("DEFAULT_IMG_HEIGHT,100);
define("DEFAULT_BG_COLOR","red");
// This is the default BACKGROUND color of the image.
// Enter a string, from the available colors shown in
// the usage section of the documentation
// Put it in quotes
// Example: define("DEFAULT_BG_COLOR","orange");
define("DEFAULT_FONT_COLOR","white");
// This is the default FONT color of the image.
// Enter a string from the available colors.
// Put it in quotes
// Example: define("DEFAULT_FONT_COLOR,"black");
define("DEFAULT_FONT_SIZE",12);
// This is the default font size in pixels.
// 12 is a normal text document size.
// Enter an integer without quotes.
// Example: define("DEFAULT_FONT_SIZE",15);
define("DEFAULT_VALIGN","middle");
// This is the vertical align of the text
// i.e. how far the top of the text is from the image's top
// Enter a string of "top", "middle", or "bottom"
// Example: define("DEFAULT_VALIGN","top");
define("DEFAULT_ALIGN","center");
// This is the x-alignment of the text
// i.e. where the text is placed on the x-axis
// Enter a string of "left","center", or "right"
// Example: define("DEFAULT_ALIGN","left");
define("FONT_PATH","/home/drew010/www/scripts/gd/fonts");
//define("FONT_PATH","/home/drew010/www/scripts/gd/fonts");
// If you are unaware of the font path on your system, download the font
// pack that is distributed with this script from www.drew-phillips.com/scripts
// Unzip them to a folder on your server and put the path above.
// Put it in quotes, NO trailing slash.
// Example: define("FONT_PATH","/home/username/www/scripts/fonts");
define("DEFAULT_TTF_FILE","luxisr.ttf");
// This is the default font file to use.
// It is located in the FONT_PATH folder. It must be a ttf file.
// Below are the available colors, you may add your own but follow the syntax
// explained in the usage section of the documentation.
$color = array("black" => "000000000", "blue" => "000000255", "crimson" => "220206000",
			 "cyan" => "000255255", "gold" => "255215000", "gray" => "128128128",
			 "green" => "000255000", "indigo" => "075000130", "lavender" => "230230250",
			 "lightblue" => "173216230", "lightgreen" => "144238144", "lightgrey" => "211211211",
			 "magenta" => "255000255", "navy" => "000000128", "olive" => "128128000",
			 "orange" => "255165000", "pink" => "255192203", "purple" => "128000128",
			 "red" => "255000000", "silver" => "192192192", "tan" => "210180140",
			 "violet" => "238130238", "white" => "255255255", "yellow" => "255255000");
######################### END CONFIGURATION ##############################
###### No editing below this line unles you know what you are doing ######
foreach($_REQUEST as $name => $value) {
	$$name = $value;
}
if(!isset($width)) {
	$width = DEFAULT_IMG_WIDTH;
}
if(!isset($height)) {
	$height = DEFAULT_IMG_HEIGHT;
}
if(!isset($font_face) || empty($font_face)) {
	$font_face = DEFAULT_FONT_FACE;
}
if(!file_exists($font_face)) {
	$font_face = DEFAULT_TTF_FILE;
}
if(!isset($font_size)) {
	$font_size = DEFAULT_FONT_SIZE;
}
#########################################################
##								  ##
## Get the rgb values for bg_color      ##
##									 ##
#########################################################
$bg_color = strtolower($bg_color);				 #
if(empty($bg_color) || !isset($bg_color)) {		 #
	$bg_color = DEFAULT_BG_COLOR;				 #
}									 #
									 #
if(!array_key_exists($bg_color,$color)) {			 #
	$bg_color = DEFAULT_BG_COLOR;				 #
}									 #
$bg_rgb = $color[$bg_color];					 #
$bg_r = $bg_rgb[0] . $bg_rgb[1] . $bg_rgb[2];		 #
$bg_g = $bg_rgb[3] . $bg_rgb[4] . $bg_rgb[5];		 #
$bg_b = $bg_rgb[6] . $bg_rgb[7] . $bg_rgb[8];		 #
#################### DONE ###############################
#########################################################
##									 #
## Get the rgb values for font_color     #
##									 #
#########################################################
$font_color = strtolower($font_color);			 #
if(empty($font_color) || !isset($font_color)) {		 #
	$font_color = DEFAULT_FONT_COLOR;			 #
}									 #
									 #
if(!array_key_exists($font_color,$color)) {		 #
	$font_color = DEFAULT_FONT_COLOR;			 #
}									 #
$ft_rgb = $color[$font_color];				 #
$ft_r = $ft_rgb[0] . $ft_rgb[1] . $ft_rgb[2];		 #
$ft_g = $ft_rgb[3] . $ft_rgb[4] . $ft_rgb[5];		 #
$ft_b = $ft_rgb[6] . $ft_rgb[7] . $ft_rgb[8];		 #
#################### DONE ###############################
if(preg_match("/\x0A/",$text)) {
	$strings = preg_split("/\x0A/",$text);
	$multipleLines = TRUE;
	$lines = count($strings);
	$textHeight = $font_size * $lines;
}
if(empty($valign) || !isset($valign)) {
	$valign = DEFAULT_VALIGN;
}
if($valign == "top") {
	$py = 5 + $font_size;
} elseif($valign == "bottom") {
	if($multipleLines == TRUE) {
		$py = $height - ($textHeight - ($font_size * $lines));
	} else {
	$py = $height - 5;
	}
} else {
	$py = ($height / 2) + ($font_size / 2);
}
if(empty($align) || !isset($align)) {
	$align = DEFAULT_ALIGN;
}
if($align == "left") {
	$px = 5;
} elseif($align == "right") {
	$px = $width - (strlen($text) * $font_size) / 2;
} else {
	if($multipleLines == TRUE) {
		$s_length = 0;
		foreach($strings as $string) {
			$cur_length = strlen($string);
			if($cur_length > $s_length) {
				$s_length = $cur_length;
			}
		}
	$px = ($width / 2) - ($font_size * ($s_length / 4));
	}
	 else {
	$px = ($width / 2) - ($font_size * (strlen($text) / 4));
	}
}
header("Content-type: image/png");
$string = $text;
$im = imagecreate($width,$height);
$background_color = imagecolorallocate($im,$bg_r,$bg_g,$bg_b);
$font_color = imagecolorallocate($im, $ft_r, $ft_g, $ft_b);
imagettftext($im,$font_size,0,$px,$py,$font_color, FONT_PATH . "/" . $font_face,$string);
imagepng($im);
imagedestroy($im);
?>
```

