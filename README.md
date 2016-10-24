# I Can Variable Font (Notes on generating variable fonts)

Notes and tips for generating a simple variable font on a Mac. These assume you’re already familiar with the UFO format, creating masters that can be interpolated, and have some comfort using Terminal.

There are probably better ways of doing this. If anyone has better sources, corrections, or the process changes, please send a pull request, [open an issue](https://github.com/scribbletone/i-can-variable-font/issues), or email us([us@scribbletone.com](us@scribbletone.com)). Hopefully this is just an interim document until the process becomes streamlined.

## Example Files
In the `example` directory, you can find a sample DesignSpace file and interpolatable UFO’s. You can use those files for your first attempt, to simplify the process, and make sure everything works. It only contains one glyph, an `A`, which is just a rectangle that should get taller and shorter.

## How To 
1. Install `fontmake`
  - First [clone or download the repository](https://github.com/googlei18n/fontmake).
  - In Terminal, navigate to the new `fontmake` repository you just downloaded.
  - Follow the instructions in their [readme](https://github.com/googlei18n/fontmake).
2. Create a DesignSpace file
  - create a new text file called `yourfont.designspace`
  - Populate the file use the following examples as a guide. Most importantly, make sure the paths to the UFOs are correct. https://github.com/scribbletone/i-can-variable-font/blob/master/example/varibox.designspace and https://github.com/LettError/MutatorMath/blob/master/Docs/designSpaceFileFormat.md
  - Add at least one instance
3. Generate interpolatable TTFs
  - In Terminal, navigate to the `fontmake` directory.
    - If you’ve closed the Terminal window since installing, you’ll also need to run `source env/bin/activate`.
  - run `fontmake -o ttf-interpolatable -m path-to-your-designspace-file`. 
    - Make sure to substitute your path to the DesignSpace file.
  - If all goes well, you should now have TTFs in the `fontmake/master_ttf_interpolatable` directory.
4. Generate the final variable font
  - Copy the generated TTFs from the previous step, and place them in the same directory as your source UFOs.
  - Make sure the TTFs have the same file name as your UFOs. If not, you’ll get an error. 
  - From the `fontmake` directory run `python env/lib/python2.7/site-packages/fontTools/varLib/__init__.py path-to-your-designspace-file`. 
    - Again, make sure to substitute your path to the DesignSpace file.
  - Cross your fingers :)
  - If everything goes well, you should end up with a new TTF file next to the DesignSpace with `-GX` in the name.

## Weird Things
- Your sources seem to need a `GPOS` or kerning table. In the example file, I got around that by just creating a single kerning pair with a value of 0.
- If you don’t have groups in your UFO, don’t include `<groups copy="1"/>` in your DesignSpace file. It’ll throw an error.

## ‘Using’ the fonts
- Mac previewer(requires running a script to build the application) https://github.com/googlei18n/fontview
- Webkit nightly https://webkit.org/downloads/

## Helpful Resources and Articles
- [Fontmake](https://github.com/googlei18n/fontmake) by Google
- [FontTools](https://github.com/fonttools) a community project with contributions by [@justvanrossum](https://github.com/justvanrossum), [@behdad](https://github.com/behdad), [@anthrotype](https://github.com/anthrotype), [@brawer](https://github.com/brawer), and many more. 
  - In particular [varLib](https://github.com/fonttools/fonttools/blob/master/Lib/fontTools/varLib/__init__.py#L13-L17)
- [MutatorMath](https://github.com/LettError/MutatorMath) by [LettError](http://letterror.com/)
- [OpenType Font Variations Overview](https://www.microsoft.com/typography/otspec180/otvaroverview.htm) by Microsoft
- [Introducing OpenType Variable Fonts](https://medium.com/@tiro/https-medium-com-tiro-introducing-opentype-variable-fonts-12ba6cd2369#.imv0hzmro) by [John Hudson](http://www.tiro.com/)
- [Variable Fonts](http://typographica.org/on-typography/variable-fonts/) by [CJ Dunn](http://thecjdunn.com/)

## Examples/Demos
- http://cjtype.com/dunbar/variablefonts/index.html
- http://stuff.djr.com/variable-demo/
