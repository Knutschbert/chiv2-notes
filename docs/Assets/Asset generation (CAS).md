 
In this case, we are using [UEAssetToolkitGenerator](https://github.com/LongerWarrior/UEAssetToolkitGenerator) to pre-process the data so it can be imported using the [UEAssetToolkit](https://github.com/Buckminsterfullerene02/UEAssetToolkit-Fixes/tree/fbx-only) editor plugins.

I strongly suggest reading [the Wiki pages](https://github.com/LongerWarrior/UEAssetToolkitGenerator/wiki) on CAS, especially about Generation process. _The process itself is very error prone. You **WILL** experience crashes and have to either try to fix the problematic parts in code or skip the problematic assets._

# Preparing the client data
## UnPAKing the game
We can use the UnPak tool provided with Unreal Engine. I’m using a wrapper for it. it can be found on this page https://gbatemp.net/threads/how-to-unpack-and-repack-unreal-engine-4-files.531784/. Follow the instructions and use v9 as the engine version.

```bash
unpack-v9.cmd "<Chivalry2>\TBL\Content\Paks\pakchunk0-WindowsNoEditor.pak"
```

The extraction process takes only a couple of minutes (on SSD). It will generate a filelist under ‘lista.txt’ and extract the data. You should see newly created _Engine_ and _TBL_ folders.
### Comparing SDK and original files
Misc command toextract .uasset names: ``grep ".uasset" lista.txt |cut -d "\"" -f 2 | sort > assetlist.txt``

Same for SDK content folder ``find "*.uasset" Content/ -type f | sort > out.txt``
Get not extracted: `grep -F -x -v -f out.txt assetlist.txt`


```

```
```


```

### Using the AssetGenerator commandlet

We can use the UE4Editor cmd interface to call the Asset Generator plugin.
It’s faster than importing inside the Editor for some types.

```bash
"I:\Epic Games\UE_4.25\Engine\Binaries\Win64\UE4Editor-Cmd.exe" "I:\chiv\CHIVSDK\TBL.uproject" -run=AssetGenerator -DumpDirectory="I:\chiv\unpacked\Output_textures" -AssetClassWhitelist="Font, FontFace" -NoRefresh -abslog="I:\chiv\AssetGenerator.log" -stdout -unattended -NoLogTimes
```

#### Get custom class names
```
grep -aioP "Class\'/Script/TBL.{0,100}\'" AssetRegistry.bin | cut -d "'" -f 2 | sort -u
```
