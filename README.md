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
3. the standard deviations of popu[Uploading 23855436.py…]()
lations in the identified age group (OP1), across all SA2 areas in this SA3 area. The second list is similar to the first one, but for input IP5 (sa2_2).

OP3: Output will be a list of list(s). Each inner list corresponds to a unique state in the
data, including three elements in the order below:
  1. the state name,
  2. the name of the SA3 area with the largest population in the identified age group(OP1), in the state,
  3. the percentage of the population which you found above with respect to the total population across all age groups in the same SA3 area. The inner list(s) should be sorted in alphabetically ascending order by the state name.
Hint: Look for sort() or sorted() function. When there are multiple areas with the same largest population, choose the first one in alphabetical order in terms of area code.

OP4: Output will be a float number which is the correlation coefficient between the populations in each age group in the first input IP4 (sa2_1), and the second input IP5 (sa2_2).

[Uploading 24305481.py…]()

def importcsv(filename, delimiter):
    #reading file and stores data in lowercase lists
    file = open(filename, 'r')
    data = []  
    headers = file.readline().strip().split(delimiter)
    headers = [h.lower() for h in headers]
    
    for line in file:
        linedata = line.strip().split(delimiter)
        linedata = [i.lower() for i in linedata]
        data.append(linedata)  
    
    file.close()
    return headers, data

#OP1
def find_age_range(age):
    #find where age lies and add/subtract 5 to get LB and UP
    if age >= 85:
        return [85, None]
    
    lower_bound, upper_bound = 0, 4
    while lower_bound < 85:
        if lower_bound <= age <= upper_bound:
            
            return [lower_bound, upper_bound]
        lower_bound += 5
        upper_bound += 5
    
    return [None, None]

#OP2
def double_list(headers, data, sa2_code, age_range):
    #slicing sa2 to get sa3
    sa2_code = str(sa2_code)
    sa3_code = sa2_code[:5] 
    
    #find the column corresponding to OP1 (age range)
    index = None
    lower_bound, _ = age_range
    for i in range(2, len(headers)):
        if headers[i].startswith('age ' + str(lower_bound)):
            index = i
            break
    
    if index is None:
        return [sa3_code, 0, 0]
    
    populations = []
    
    #collecting population, if first 5 digits same, append pop count
    for row in data:
        if row[0][:5] == sa3_code:
            populations.append(int(row[index]))
    
    if not populations:
        return [sa3_code, 0, 0]
    
    #mean
    total = 0
    count = len(populations)
    for value in populations:
        total += value
    mean = total / count
    
    #variance
    variance_sum = 0
    for value in populations:  
        difference = value - mean  
        squared_difference = difference ** 2  
        variance_sum += squared_difference 

    #standard deviation
    if count > 1:  
        variance = variance_sum / (count - 1)  
        std_dev = variance ** 0.5
    else:
        std_dev = 0  

    return [sa3_code, round(mean, 4), round(std_dev, 4)]

# OP3
def find_name(code, areas_data, index): 
    #index of age group column
    for item in areas_data:
        if int(item[index]) == int(code):
            return item[index + 1] #second column

    return None 

def process_population_data(areas_data, population_data, OP1):
    prev_state_code = -1 #intiitalize as -1 so u continue to compare to prev data
    curr_sa3_code = ''
    max_sa3_code = ''
    sa2_pop_sum = 0
    max_sa3_pop = 0
    results = []
    index = OP1[0] // 5 + 2
    total_pop_all_sa3s = 0
    max_pop_all_sa3s = 0

    # go thru each row of pop data & taking state code from first digit of sa2 code
    for i in range(len(population_data)):
        if int(population_data[i][0][0]) != prev_state_code: 
            if i != 0: #If we on new state
                result = float(max_sa3_pop) / float(max_pop_all_sa3s) #calc proportion of pop
                results.append([
                    find_name(prev_state_code, areas_data, 0), 
                    find_name(max_sa3_code, areas_data, 2), 
                    round(result, 4)
                ])

                max_sa3_pop = 0
                total_pop_all_sa3s = 0

            #updating initialized value
            prev_state_code = int(population_data[i][0][0])
            curr_sa3_code = population_data[i][0][0:5] #slice for first 5 digi
            sa2_pop_sum = int(population_data[i][index])
            total_pop_all_sa3s += sum(int(x) for x in population_data[i][2:])
    
        else: #if we on same state
            #if same sa3, keep adding pop to the current sa3 total
            if population_data[i][0][0:5] == curr_sa3_code:
                sa2_pop_sum += int(population_data[i][index])
                total_pop_all_sa3s += sum(int(x) for x in population_data[i][2:])
            else:
                #if new sa3 area, compare values to find max
                if max_sa3_pop < sa2_pop_sum:
                    max_sa3_pop = sa2_pop_sum
                    max_sa3_code = curr_sa3_code
                    max_pop_all_sa3s = total_pop_all_sa3s

                sa2_pop_sum = int(population_data[i][index])
                curr_sa3_code = population_data[i][0][0:5]
                total_pop_all_sa3s = sum(int(x) for x in population_data[i][2:])
    
    results.append([
        find_name(prev_state_code, areas_data, 0), 
        find_name(max_sa3_code, areas_data, 2), 
        round((max_sa3_pop / max_pop_all_sa3s), 4)
    ])

    results.sort()
    return results


def correlation(pop, sa21, sa22):
    #dic to loop sa2 data by sa2 code
    dic = {}
    for row in pop:
        dic[row[0]] = row

    #taking pop data
    line = dic.get(sa21)
    line2 = dic.get(sa22)

    #extracting integers
    data = [int(x) for x in line[2:]]
    data2 = [int(x) for x in line2[2:]]

    #mean
    avg = sum(data) / len(data)
    avg2 = sum(data2) / len(data2)

    #correlation
    numerator = 0
    denom1 = 0
    denom2 = 0
    for x, y in zip(data, data2):
        dx = x - avg
        dy = y - avg2
        numerator += dx * dy
        denom1 += dx * dx
        denom2 += dy * dy

    return round((numerator / (denom1 * denom2) ** 0.5), 4)

def main(csvfile_1, csvfile_2, age, sa2_1, sa2_2):
    try:
        headers1, areas_data = importcsv(csvfile_1, ',')
        headers2, population_data = importcsv(csvfile_2, ',')
        OP1 = find_age_range(age)
        OP2 = [double_list(headers2, population_data, sa2_1, OP1), double_list(headers2, population_data, sa2_2, OP1)]
        OP3 = process_population_data(areas_data, population_data, OP1)
        OP4 = correlation(population_data, sa2_1, sa2_2)
        
        return OP1, OP2, OP3, OP4
    
    except ZeroDivisionError:
        print('cant divide by 0')
        return None, [], [], 0
    except:
        print('error')




