# Toasties-UTS
Mr Toasties Universal Tileset Standard
Overview

This standard defines a tileset stored as a single PNG file that contains:

    A Sample Room Image: A standardized, overhead view of a dungeon room demonstrating how the tiles connect (e.g., floor, walls, doorways).

    Embedded Metadata: All information about individual tiles (their types, variations, and collision data) is stored in the PNG’s metadata. This metadata (typically stored in tEXt/iTXt chunks) is encoded as JSON.

PNG File Structure

    Image Content:
    The PNG image should display a complete sample room (or level section) arranged using the tiles. This image shows how the tiles can be combined to form a dungeon layout.

    Metadata:
    The image includes a metadata block that defines:

        Tileset Version: Version of the tileset standard.

        Tile Grid Information: Size of each tile (e.g., 32×32 pixels) and the layout of the sample room (optional, if needed for reference).

        Tile Types: Definitions for each type of tile, including common ones like "Floor," "West wall," "East wall," "North wall," "South wall," etc.

Tileset Metadata Format

The metadata should be stored as a JSON object. Here’s an example schema:

{
  "tilesetVersion": "1.0",
  "tileSize": { "width": 32, "height": 32 },
  "tiles": {
    "Floor": {
      "description": "Standard floor tile.",
      "variations": [
        { "id": 1, "sourceRect": [x1, y1, width, height] },
        { "id": 2, "sourceRect": [x2, y2, width, height] }
      ],
      "collision": {
        "id": "Floor_collide",
        "collisionMap": "data:image/png;base64,..."
      }
    },
    "WestWall": {
      "description": "Tile for the west wall.",
      "variations": [
        { "id": 1, "sourceRect": [x3, y3, width, height] },
        { "id": 2, "sourceRect": [x4, y4, width, height] }
      ],
      "collision": {
        "id": "WestWall_collide",
        "collisionMap": "data:image/png;base64,..."
      }
    }
    // ... additional tile types (e.g., EastWall, NorthWall, SouthWall, etc.)
  },
  "usageInstructions": "To render the dungeon, use the sample room image as a guide. For each tile placement, randomly select one of the defined variations for that tile type. For collision detection, use the associated collision maps, where white pixels represent collision (solid) and black pixels represent passable areas."
}

Explanation of Fields

    tilesetVersion:
    A string indicating the version of your tileset standard.

    tileSize:
    Defines the pixel dimensions of each tile in the tileset.

    tiles:
    A mapping of tile type names (e.g., "Floor", "WestWall") to their definitions.

        description:
        A human-readable explanation of the tile type.

        variations:
        An array of objects, each representing a different visual variant of the tile.

            id: A numeric (or string) identifier for the variant.

            sourceRect: Coordinates in the PNG (x, y, width, height) that define where this tile is located. This allows your tile engine to extract the proper section of the image.

        collision:
        An object specifying the collision version of the tile.

            id: An identifier for the collision tile.

            collisionMap: A Base64-encoded PNG (or similar) that represents the collision data. In this image, white indicates a colliding region (solid), and black indicates a non-colliding region.

    usageInstructions:
    A text field that explains how to use the tiles and metadata, including how to randomly select among variations and how to interpret the collision maps.

Suggestions and Improvements

    Standardize the Metadata Key:
    Define a specific key (e.g., "UTS_Tileset") in the metadata chunk so that tools and engines know where to look for tileset data.

    Consider a JSON Schema:
    Create and publish a JSON Schema for this metadata. This helps users validate their metadata against the standard.

    Uniform Grid Coordinates:
    If your tiles are arranged uniformly in the image, consider automatically generating the sourceRect values based on tile size and grid position. This reduces manual entry errors.

    Support for Additional Properties:
    You might later include properties such as:

        Animation data: If some tiles have animated variations.

        Weighting: Values that indicate the likelihood of each variation being selected.

        Layering information: For cases where tiles overlap.

    Collision Map Compression:
    If collision maps are simple black-and-white images, consider compressing them or storing them as binary data (Base64) to reduce metadata size.

    Tooling:
    Create or recommend tools/scripts that can automatically generate or update the metadata based on a tileset image (for instance, by slicing the image into a grid).

Final Thoughts

This standard provides a way to package both the visual tiles and the associated usage instructions/collision data in one portable PNG file. It leverages the flexibility of PNG metadata and the widespread use of JSON for configuration. As you refine your project, you might find additional fields or adjustments are needed; the above format is a solid foundation that is both human-readable and machine-parsable.