# BlenderQuertyToDvorak
Blender 2.8 keyboard import script for mapping physical QWERTY keyboard to a Dvorak

Keyboard import script: https://github.com/vpicaver/BlenderQuertyToDvorak/blob/master/QwertyToDvorak_2.8.py

For example on a QWERTY keyboard, Blender maps `File->Save` to `Control-S`. This script will remap it to `Control-O`. 

This is how the file was generated to map Qwerty to Dvorak in Blender 2.8:

1. Find `modules/bl_keymap_utils/io.py`
On MacOS, mine is located here: `/Applications/blender.app/Contents/Resources/2.80/scripts/modules/bl_keymap_utils/io.py`

2. In `io.py` find `def keyconfig_export_as_data(...)` function

3. Add `all_keymaps=True` before any of the code runs in `keyconfig_export_as_data()`
This will force Blender to export all the keyboard short cuts

4. Start Blender, or restart it if you already have it open

5. Export all the keyboard to a python file shortcuts in Edit->Preferences->Keymap  

6. Scroll to the bottom of the exported python file (mine over 10,000 lines long) and replace:

    if __name__ == "__main__":
        import os
        from bl_keymap_utils.io import keyconfig_import_from_data
        keyconfig_import_from_data(os.path.splitext(os.path.basename(__file__))[0], keyconfig_data)

with

    q_to_d = {
         'MINUS': 'LEFT_BRACKET',
         'EQUAL': 'RIGHT_BRACKET',
         'Q': 'QUOTE',
         'W': 'COMMA',
         'E': 'PERIOD',
         'R': 'P',
         'T': 'Y',
         'Y': 'F',
         'U': 'G',
         'I': 'C',
         'O': 'R',
         'P': 'L',
         'LEFT_BRACKET': 'SLASH',
         'RIGHT_BRACKET': 'EQUAL',
     #    'BACK_SLASH': 'BACK_SLASH',
     #    'A': 'A',
         'S': 'O',
         'D': 'E',
         'F': 'U',
         'G': 'I',
         'H': 'D',
         'J': 'H',
         'K': 'T',
         'L': 'N',
         'SEMI_COLON': 'S',
         'QUOTE': 'MINUS',
         'Z': 'SEMI_COLON',
         'X': 'Q',
         'C': 'J',
         'V': 'K',
         'B': 'X',
         'N': 'B',
     #    'M': 'M',
         'COMMA': 'W',
         'PERIOD ': 'V',
         'SLASH': 'Z',
    }

    if __name__ == "__main__":
        import os
        from bl_keymap_utils.io import keyconfig_import_from_data
    
        def remap(keys):
            if isinstance(keys, (list, tuple, dict)):
                for k in keys:
                    if isinstance(k, (list, tuple)):
                        remap(k)
                    elif isinstance(k, dict):
                        #print(type(k))
                        #print(k)
                        
                        if 'type' in k:
                            dvorakKey = q_to_d.get(k["type"], None)
                            if dvorakKey != None:
                                print(f'Remapping {k["type"]} -> {dvorakKey}')
                                k["type"] = dvorakKey
                        else:
                            for (dicKey, dicValue) in k.items():
                                remap(dicValue)
                            
        remap(keyconfig_data)
    
    
    keyconfig_import_from_data(os.path.splitext(os.path.basename(__file__))[0], keyconfig_data)

7. Save and re-import the saved file.

