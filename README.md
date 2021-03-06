# 3dsconv
`3dsconv.py` is a Python 2.7 script that converts Nintendo 3DS CTR Cart Image files (CCI, ".cci", ".3ds") to the CTR Importable Archive format (CIA).

3dsconv can detect if a CCI is decrypted or encrypted, or zerokey-encrypted. Decrypted is preferred and does not require any extra files. Encrypted requires Extended Header (ExHeader) XORpads, with the filename `<TITLEID>.Main.exheader.xorpad`. Zerokey encryption works only if [PyCrypto](https://www.dlitz.net/software/pycrypto/) is installed (can be installed with pip).

This does not work with Python 3.

[Decrypt9WIP](https://github.com/d0k3/Decrypt9WIP) can dump game cards to CIA directly now, rendering this tool partially obsolete. It can still be used for existing game dumps, however.

## Usage
### Basic use
On Windows, decrypted CCIs can be dragged on top of `3dsconv.exe`. Encrypted CCIs should be decrypted, or have the proper ExHeader XORpads in the same folder.

### Advanced options
```bash
python2 3dsconv.py [options] game.3ds [game.3ds ...]
```
* `--xorpads=<dir>` - use XORpads in the specified directory  
  default is current directory or what is set as `xorpad-directory`
* `--output=<dir>` - save converted CIA files in the specified directory  
  default is current directory or what is set as `output-directory`
* `--overwrite` - overwrite any existing converted CIA, if it exists
* `--gen-ncchinfo` - generate `ncchinfo.bin` for CCIs without a valid xorpad
* `--gen-ncch-all` - generate `ncchinfo.bin` for all CCIs
  used with --gen-ncchinfo
* `--noconvert` - don't convert CCIs, useful to generate just `ncchinfo.bin`
* `--ignorebadhash` - ignore bad xorpad/corrupt rom and convert anyway
* `--verbose` - print more information

## Generating XORpads
If your CCI is encrypted, you must generate ExHeader XORpads with a 3DS system and the ability to use [Decrypt9](https://github.com/d0k3/Decrypt9WIP).

1. Use `--gen-ncchinfo` with the CCIs you want to generate them for.  
   By default, only CCIs without valid XORpads will be added into `ncchinfo.bin`. To add all given CCIs, add `--gen-ncch-all`.
2. Place `ncchinfo.bin` at the root or `/Decrypt9` on your 3DS SD card.
3. Run Decrypt9, and go to "XORpad Generator Options" and "NCCH Padgen".

XORpads can then be placed anywhere (use `--xorpads=<dir>` if it's not the current working directory)

## Converting .py to standalone .exe (Windows)
Using [py2exe](http://www.py2exe.org/), you can pack the script into a Windows executable, for use on a computer without Python, or for easy use in the Windows command prompt.

1. Clone or download the repository, or the latest release + `setup.py` from the repository.
2. Open the Windows command prompt (`cmd.exe`) in the current directory.
3. Edit `setup.py` to change any options, if wanted.
4. Run `python setup.py py2exe`. Make sure Python 2.7 is being used.
5. `3dsconv.exe` and its dependencies will be in `dist` after it finishes.

## License / Credits
* `3dsconv.py` is under the MIT license.
* `ncchinfo.bin` generation is based on [Decrypt9WIP's `ncchinfo_gen.py`](https://github.com/d0k3/Decrypt9WIP/blob/master/scripts/ncchinfo_gen.py).
* Zerokey crypto code is based off [this ncchinfo padgen pastebin](http://pastebin.com/K3pVsnkq).

For versions older than "2.0", see this [Gist](https://gist.github.com/ihaveamac/dfc01fa09483c275f72ad69cd7e8080f).
