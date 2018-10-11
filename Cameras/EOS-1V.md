# Canon EOS-1V Metadata fix

The following commands require Exiftool to be installed. Exiftool can be downloaded [here](https://www.sno.phy.queensu.ca/~phil/exiftool/). 
The commands are meant to fix incomplete or incorrect exif data after images have been processed with Meta35. 

## Introduction
The EOS-1V bundled with a Meta35 adapter and its software makes recording and then matching exif data with images files very easy. 
However, compared with the digital EOS cameras, the camera model tag is not in line with what digital EOS cameras write to their files. 

Since the EOS-1V doesn't record lens model information, Meta35 is unable to write that data to image files. 
The lens related commands let you, with knowledge of what lenses you were using at the time, supplement that information. 

Based on the data available to you, this unfortunately doesn't enable you to make a certain decision at times. 
Let's say you're using a 16-35mm f/2.8 zoom lens and a 24-70mm f/2.8 zoom lens. 
The 24-35mm range being covered by both lenses at a max aperture of f/2.8 won't allow you to make a certain decision. 

If you're only using lenses that do not overlap focal lenght wise, you don't have to double check or worry about any of this. You'll be able to make an accurate decision every time. 

Exiftool usually works very well. As with all digital files, things can always go wrong for unforeseen reasons. Please back up important files before processing them. 

## Commands

Make sure to replace the `dir` variable at the end of every command with the directory that the image files are located in. 
The following commands are all tested on mac OS 10.14. Other Unix systems should work without any problems. On Windows, you'll have to replace certain characters. Check Exiftool's documentation. 

### Camera Make and Model tag fix

Changes the follwing tags: 
- Make
- Model

Run the following command to fix the camera make and model: 

`exiftool -Make="Canon" -Model="Canon EOS-1V" dir`

### Lens 

The checking commands return the matched lens for the numerical LensID value. If you'd like to see the numerical value, simply add a # to the LensID tag to make the command look like this: 

`exiftool -LensID# -LensInfo -LensModel -LensSerialNumber -if '$LensID# eq "751"' dir`

Changes the following tags: 
- LensID
- Lens
- LensInfo
- LensModel
- LensSerialNumber

#### Creating your own custom command for any lens 

The next section contains sample commands for two lenses and a lens plus extender combination. 
To create a command for any other lens, please look up the numerical value for other lenses [here](https://sno.phy.queensu.ca/~phil/exiftool/TagNames/Canon.html#LensType). You can also find this information in files captured on digital EOS cameras by running the following command: 

`exiftool -LensID -Lens -LensInfo -LensModel -LensSerialNumber dir`

The template for your command looks like this: 
First you want to write the LensID to your file. Specify a **zoom range** for the lens you were using by replacing **lowVal** and **upperVal**. Then replace **LensIdVal** with your LensID value in the following command: 

`exiftool -LensID="LensIdVal" -if '$FocalLength# >= "lowVal" && $FocalLength# <= "upperVal"' dir`

For a **prime lens**, replace **LensIdVal** with your LensID value and **FlVal** with the FocalLength value the following command: 

`exiftool -LensID="LensIdVal" -if '$FocalLength# == "lowVal"' dir`

Replace **lensStringVal**, **lensInfoVal**, **lensModVal**, **serialNumberVal**, and **LensIdVal** with your own values. 

`exiftool -Lens="lensStringVal" -LensInfo="lensInfoVal" -LensModel="lensModVal" -LensSerialNumber="serialNumberVal" -if '$LensID# eq "LensIdVal"' dir`

#### Canon EF16-35mm f/2.8L III USM

Write missing LensID to file

`exiftool -LensID="751" -if '$FocalLength# >= "16" && $FocalLength# <= "35"' dir`

Write remaining missing info to 1V files

`exiftool -Lens="EF16-35mm f/2.8L III USM" -LensInfo="16-35mm f/0" -LensModel="EF16-35mm f/2.8L III USM" -LensSerialNumber="insert your Serial number here" -if '$LensID# eq "751"' dir`

Check that they're correctly written to file

`exiftool -LensID -LensInfo -LensModel -LensSerialNumber -if '$LensID# eq "751"' dir`

Check all other files 

`exiftool -LensID -LensInfo -LensModel -LensSerialNumber -if '$LensID# ne "751"' dir`


#### Canon EF 100-400mm f/4.5-5.6L IS II USM

Write missing LensID to file

`exiftool -LensID="747" -if '$FocalLength# >= "100" && $FocalLength# <= "400"' dir`

Write remaining missing info to 1V files

`exiftool -Lens="EF100-400mm f/4.5-5.6L IS II USM" -LensInfo="100-400mm f/?" -LensModel="EF100-400mm f/4.5-5.6L IS II USM" -LensSerialNumber="insert your Serial number here" -if '$LensID# eq "747"' dir`

Check that they're correctly written to file

`exiftool -LensID -LensInfo -LensModel -LensSerialNumber -if '$LensID# eq "747"' dir`

Check all other files 

`exiftool -LensID -LensInfo -LensModel -LensSerialNumber -if '$LensID# ne "747"' dir`

#### Canon EF 100-400mm f/4.5-5.6L IS II USM + 1.4x Extender

Write missing LensID to file

`exiftool -LensID="748" -if '$FocalLength# >= "401" && $FocalLength# <= "560"' dir`

Write remaining missing info to 1V files

`exiftool -Lens="EF100-400mm f/4.5-5.6L IS II USM +1.4x III" -LensInfo="140-560mm f/?" -LensModel="EF100-400mm f/4.5-5.6L IS II USM +1.4x III" -LensSerialNumber="insert your Serial number here" -if '$LensID# eq "748"' dir`

Check that they're correctly written to file

`exiftool -LensID -LensInfo -LensModel -LensSerialNumber -if '$LensID# eq "748"' dir`

Check all other files 

`exiftool -LensID -LensInfo -LensModel -LensSerialNumber -if '$LensID# ne "748"' dir`