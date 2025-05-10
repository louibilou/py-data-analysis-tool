# py-data-analysis-tool
Background: (hypothetically) The Australian Bureau of Statistics (ABS) requires a powerful data analysis tool that can provide insights into the population distribution across Australia's cities and regions. The government and urban planners require this tool to make informed decisions regarding infrastructure development, resource allocation, and future urban planning. my task is to develop an analytical tool that processes real-world population datasets and generates insightful statistics.

Data: The data for this project is the population information by areas and ages, distributed in two data files. You need to find the correct data association across files. The files include the codes and names of Australian states, statistical areas (Level 2 & 3), and different age population groups living in these areas

the Python 3 program that will read two CSV files and then produce the following output: 
  1) Find the age group that contains a specific input age.
  2) Calculate population statistics for two specific SA3 areas.
  3) Find the SA3 area with the largest population in the age group, for each unique state, and its percentage.
  4) Calculate the correlation between the age structure of two specific SA2 areas.

Requirements
  1) You are not allowed to import any external or internal module in python. While use of many of these modules, e.g., csv or math is a perfectly sensible thing to do in a production setting, it takes away much of the point of different aspects of the project, which is about getting practice opening text files, processing text file data, and use of basic Python structures, in this case lists and loops.
  2) Ensure your program does NOT call the input() function at any time.
  3) Your program should also not call print() function at any time except for the case of graceful termination (if needed). If your program has encountered an error state and is exiting gracefully then your program needs to return zero (for number), None (for string), or empty list (for list) and print an appropriate message. At no point should you print the program’s outputs instead of (or in addition to) returning them or provide a printout of the program’s progress in calculating such outputs.
  4) Do not assume that the input file names will end in .csv. File name suffixes such as .csv and .txt are not mandatory in systems other than Microsoft Windows. Do not   enforce that within your program that the file must end with a .csv or any other extension (or try to add an extension onto the provided csv file argument), doing so can    easily lead to syntax error and losing marks.
     
Input
Your program must define the function main with the following syntax:
def main(csvfile_1, csvfile_2, age, sa2_1, sa2_2):

Output
OP1: Output will be a list including two integers, indicating the lower bound and the upper bound of the age group containing the input age (IP3: age). Use None as the list element if one of the bounds does not exist.
Note: The age group identified in this output will be used to calculate the other outputs.

OP2: Output will be a list of two lists. The first list includes three elements in the order below:
1. the code of the SA3 area where input IP4 (sa2_1) is located,
2. the average of populations in the identified age group (OP1), across all SA2 areas in this SA3 area,
3. the standard deviations of populations in the identified age group (OP1), across all SA2 areas in this SA3 area. The second list is similar to the first one, but for input IP5 (sa2_2).

OP3: Output will be a list of list(s). Each inner list corresponds to a unique state in the
data, including three elements in the order below:
  1. the state name,
  2. the name of the SA3 area with the largest population in the identified age group(OP1), in the state,
  3. the percentage of the population which you found above with respect to the total population across all age groups in the same SA3 area. The inner list(s) should be sorted in alphabetically ascending order by the state name.
Hint: Look for sort() or sorted() function. When there are multiple areas with the same largest population, choose the first one in alphabetical order in terms of area code.

OP4: Output will be a float number which is the correlation coefficient between the populations in each age group in the first input IP4 (sa2_1), and the second input IP5 (sa2_2).
