

# Keyboard Commands

## Modes

* Switch from Edit Mode (Green) to Command Mode (Blue): <esc>
* Switch from Command Mode (Blue) to Edit Mode (Green):  <enter>

## Navigation
The following commands  assume you are in Command Mode.  

* Move up a cell: k
* Move down a cell: j


## Adding Cells
To avoid write <esc> too many times, the commands below assume you are already in Command 
Mode.  If you are in Edit Mode, these commands work by first escaping into Command Mode w/ <esc>.

* Add cell above current cell: a
* Add cell below current cell: b
* Delete current cell: dd

## Copying, Yanking, and Pasting Cells
To avoid write <esc> too many times, the commands below assume you are already in Command 
Mode.  If you are in Edit Mode, these commands work by first escaping into Command Mode w/ <esc>.
  
* Copy: c
* Yank: x
* Paste: v

## Transmogrify Cell Type
To avoid write <esc> too many times, the commands below assume you are already in Command 
Mode.  If you are in Edit Mode, these commands work by first escaping into Command Mode w/ <esc>.
  
* Change to MarkDown Cell: m

## Mathematical Typesetting
Bascially: LaTex in a MarkDown cell.  

* inline math: $\dot{x}=f(x)$
* math section: $$\dot{x}=f(x)$$
* environments: \begin{envName} ... \end{endName}
  - gather
  - align

For a set of equations, you must specify an environment like `gather` or `align`,

Question: Why doesn't GitHub's MarkDown have LaTeX functionality?  So annoying... Basically, you 
have to use a service to generate images (e.g., https://www.codecogs.com/latex/eqneditor.php).





