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

<br>

## Technical Details
Patch files was generated automatically using a few (linux) commands:  

1. **Heading to game directory:**

    ```bash
    $ cd /path/to/Starbound/
    ```

2. **Creating** `_template.consumable.patch` **file with contents:**

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

3. **Unpacking Starbound assets:**

    ```bash
    $ ./linux/asset_unpacker "./assets/packed.pak" "./_starbound_assets"
    ```

4. **Creating** `_starbound_consumables` **directory:**
    ```bash
    $ mkdir -p "./_starbound_consumables"
    ```

5. **Copying** `.consumable` **files to** `starbound_consumables`**:**

    ```bash
    $ find "./_starbound_assets" -name "*.consumable" -print -exec cp --parents "{}" "./_starbound_consumables" \;
    ```

6. **Renaming** `*.consumable` **to** `.consumable.patch`**:**

    ```bash
    $ find "./_starbound_consumables" -name "*.consumable" -exec mv "{}" "{}.patch" \;
    ```

7. **Overriding** `.consumable.patch` **files with contents of** `./_template.consumable.patch`**:**

    ```bash
    $ find "./_starbound_consumables" -name "*.consumable.patch" -exec cp "./_template.consumable.patch" "{}" \;
    ```
    
8. (Optional) **Copying files to mod repository and cleaning up workspace:**

    ```bash
    $ cp -Ri "./_starbound_consumables/_starbound_assets/" "/path/to/open_food_mechanics/"
    $ rm -R "./_starbound_assets/" "./_starbound_consumables/" "./_template.consumable.patch"
    ```
