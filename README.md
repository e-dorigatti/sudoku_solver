This repository contains an end-to-end Sudoku solver. End-to-end means that the Sudoku board can be recognized from an image coming from, e.g. the computer webcam. This repository contains the following notebooks:

 - `end2end.ipynb`: this notebook contains a full demo using the code contained in the other notebooks. It records from the webcam and waits for the user to show a Sudoku, then recognizes the board and the digits, and through simple interaction asks the user to correct possible mistakes, and finally tries to solve the puzzle.
 - `find_grid_boxes.ipynb`: this is my first failed attempt at extracting the Sudoku board from an image. The idea was to extract the thick lines, then aggregate them to find the 9 large squares, and finally cut each of them into the 9 small cells; unfortunately this proved quite hard so I abandoned this approach. Perhaps better CV parameters would improve the recognition of the thick lines, and make the following tasks easier.
  - `extract_digits.ipynb`: this notebook contains the code that extracts the 81 cells from a Sudoku image. The high level approach is as follows:
     1. smooth and binarize the image, and apply further pre-processing to enhance the thick lines and remove the thin ones
     2. extract the center square using flood fill
     3. use the size and position of the center square to find the approximate position of the other 8
     4. find the remaining squares with flood fill
     5. cut all the squares in 9 pieces to find the digits
     6. finally, provide a very simple "GUI" to label the cells into 11 categories (digits + empty)
 - `recognize_digits.ipynb`: this notebook uses the digits extracted and labeled by the previous notebook to train a CNN that is able to recognize the content of a cell, as well as an utility function to predict digits using this network. This is based on Keras and uses the image data generator to augment the training set.
 - `solve_sodoku.ipynb`: this notebook contains the Sudoku solver. The solver is based on the concept of _candidates_, i.e. the possible numbers that can fill a blank cell. It relies on some simple rules to prune these candidates and fill in the easy cells, then uses a simple DFS to find the full solution. This solver is quite fast because most of the cells are filled by rules, and the DFS only needs to try few combinations.
