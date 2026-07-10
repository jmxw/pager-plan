# Design

## Draw the Design Wireframe

1. **Drawing tool** (Confluence rule: the draw.io plugin with `Insert a new draw.io diagram`. Replaced for md as follows)
   - The wireframe is drawn with draw.io desktop or the VS Code extension (hediet.vscode-drawio) and saved as `svg/<status>.drawio.svg` — draw.io's **editable SVG** format (File → Save / Export as → SVG with *"Include a copy of my diagram"* checked). This single file both renders inline in md and stays re-editable in draw.io, because it embeds the diagram source inside the SVG.
   - **One file, not two.** The `.drawio.svg` *is* the source; there is no separate raw `.drawio`. Open the `.drawio.svg` directly in draw.io / the VS Code extension to edit, and save it in place. (A raw `.drawio` is avoided because it carries no rendered geometry and would show as a broken image in MkDocs / GitHub.)
   - The file is text-based and git-diffable; prefer this SVG over the bitmap `.drawio.png`.
2. **The base is a wireframe drawn directly in draw.io** — a low-fidelity layout of the screen, not a Figma export and not an OS screenshot. There is no separate base image to import
3. **Attach all possible states of each screen** and assign a number to each item
4. Provide necessary explanations for each item
5. **If an explanation exceeds 3 lines**, document it in the [Functions](./functions.md) section

## How to Assign Numbers to Images

- Place a number label (e.g. ①, ②, ③) directly on or near the relevant part of the image
- Use consistent formatting for all numbers
- Make sure each number corresponds to a clearly explained item in the documentation
- Avoid overlapping numbers with important UI elements
- **Number components continuously across the whole doc** (No.1, No.2, … No.N), not restarting per
  status. Each number is unique in the doc and is the badge drawn on that component in the wireframe.
- **A component that appears in more than one status keeps its one number everywhere**, and is
  **described in full only at its first appearance**. A later status that reuses it shows the same
  badge number on its wireframe and, in its component table, lists that number and points back
  (e.g. "Same as No.8") instead of re-describing it. See the [login example](./example/login/README.md)

## Components to Use for Annotation

| Component                | Usage                                                                                                                                                                                                                                          |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Diagonal Area**  | Indicates areas of the screen that are not used. These sections are marked with diagonal lines                                                                                                                                                 |
| **Rectangle Area** | Highlights the target section by drawing a box around it                                                                                                                                                                                       |
| **Number Label**   | Placed near the target section to indicate reference numbers. Placement priority: 1. Top-left → 2. Bottom-left → 3. Top-right → 4. Bottom-right (this priority is not mandatory; it is a reference and can be defined per screen as needed) |
| **Clickable area** | Clickable areas are displayed in**blue**                                                                                                                                                                                                 |

## Example of the Description Table

| No | Name             | Description                                                                                |
| -- | ---------------- | ------------------------------------------------------------------------------------------ |
| 1  | Flowchart button | The button name is`流程` ← This is the default view. Click to move to [target page](url) |

## Rules to Describe Components

### About IOE

| Symbol               | Meaning                                                                                   |
| -------------------- | ----------------------------------------------------------------------------------------- |
| **I** (Input)  | Components where users can input or make selections                                       |
| **O** (Output) | Components displaying data retrieved from the API                                         |
| **E** (Event)  | Components where events occur                                                             |
| **-**          | Labels displaying fixed values on the FE side that do not fall under the above categories |

### #8 Additional rule proposals (sections missing from the page)

1. **File naming**: `svg/<status>.drawio.svg`, lowercase, the `<status>` matching the Name column of the Statuses section. The spec's own folder already scopes the screen, so the file name is just the status — no `<screen_id>_` prefix. See the login example's [`svg/`](./example/login/svg/), e.g. `select.drawio.svg`
2. **One status = one wireframe**: a `.drawio.svg` is prepared for every status listed in the Statuses section
3. **How to fill the "No Permission Display" column** (the column exists in the PRD Template Design table but is not defined anywhere): describe what is shown to a user without permission, using one of `hidden` (not rendered) / `disabled` (rendered but inactive) / `-` (no difference by permission)
4. **Update rule**: when the design changes, the wireframe must be re-drawn and the number labels / annotations reviewed in the same update
