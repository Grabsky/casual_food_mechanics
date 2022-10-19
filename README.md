# Open Food Mechanics
Starbound mod that prevents food from rotting and makes it <sup>(along with other consumables)</sup> stackable up to **20**.  

Pull requests with support for other mods are welcome. Read [Technical Details](#technical-details) to get an idea how to do that semi-automatically.

<br>

## Installation
> Always backup `path/to/Starbound/storage` directory before (un)installing (any) mod(s).  

Download latest version [here](https://github.com/Grabsky/open_food_mechanics/releases/latest) and place it inside `/path/to/Starbound/mods`.  

**Removing the mod will bring stack sizes back to normal. Extra items will be lost so make sure to unstack them before proceeding.**

<br>

## Compatibility
> No incompatibilities found, make sure to open an issue if you find one.  

**Compatible**: NeoVanAlemania's [Improved Food Descriptions](https://community.playstarbound.com/resources/improved-food-descriptions.3743/)  
**Compatible**: Niffe's [More Canned Food](https://steamcommunity.com/sharedfiles/filedetails/?id=2835491179)  

<br>

## Technical Details
Patch files were generated automatically using a few (linux) commands:  

1. **Head to the game directory:**

    ```bash
    $ cd /path/to/Starbound/
    ```

2. **Create** `_template.consumable.patch` **file with following contents:**

    ```json
    [
        [
            { "op": "test", "path": "/maxStack" },
            { "op": "replace", "path": "/maxStack", "value": 20 }
        ],
        [
            { "op": "test", "path": "/maxStack", "inverse": true },
            { "op": "add", "path": "/maxStack", "value": 20 }
        ]
    ]
    ```

3. **Unpack Starbound assets:**

    ```bash
    $ ./linux/asset_unpacker "./assets/packed.pak" "./_starbound_assets"
    ```

4. **Create** `_starbound_consumables` **directory:**
    ```bash
    $ mkdir -p "./_starbound_consumables"
    ```

5. **Copy all** `*.consumable` **files to** `starbound_consumables` **directory:**

    ```bash
    $ find "./_starbound_assets" -name "*.consumable" -print -exec cp --parents "{}" "./_starbound_consumables" \;
    ```

6. **Rename all** `*.consumable` **files to** `*.consumable.patch`**:**

    ```bash
    $ find "./_starbound_consumables" -name "*.consumable" -exec mv "{}" "{}.patch" \;
    ```

7. **Override all** `*.consumable.patch` **files with contents of** `./_template.consumable.patch` **file:**

    ```bash
    $ find "./_starbound_consumables" -name "*.consumable.patch" -exec cp "./_template.consumable.patch" "{}" \;
    ```
    
8. (Optional) **Copy files to mod repository and clean up workspace:**

    ```bash
    $ cp -Ri "./_starbound_consumables/_starbound_assets/" "/path/to/open_food_mechanics/"
    $ rm -R "./_starbound_assets/" "./_starbound_consumables/" "./_template.consumable.patch"
    ```
