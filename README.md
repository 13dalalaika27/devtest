# **There are several options here for parsing logs** :smirk:

### Prerequisites
OS Linux and access to terminal

Preferably root access to the server and the file you need to work with or you can use the existing [nginx.log](https://github.com/13dalalaika27/devtest/blob/main/nginx.log)

## So, the options that can be used :star:

1. You can use it for sorting by the number of requests from certain IPs:

(We will use in the example, the output of the information  in the file **result.txt** (or in any other format as needed, for example, csv))
```
cat path_to_the_directory/nginx.log | awk '{print $1}' | sort | uniq -c | sort > result.txt
```
2. List of files (links) that were requested:
```
cat path_to_the_directory/nginx.log  | awk '{ print $7}' | sort | uniq -c | sort > result.txt
```
3. Number of requests per secondЖ
```
cat path_to_the_directory/nginx.log  | awk '{print $4}' | uniq -c | sort -rn | head > result.txt 
```
4. Response code sorting:
```
cat path_to_the_directory/nginx.log | cut -d '"' -f3 | cut -d ' ' -f2 | sort | uniq -c | sort -rn > result.txt
```

:arrow_right: [Results you can see here](https://github.com/13dalalaika27/devtest) :arrow_left: 
(According to the numbering of the files named 1 to 4 with the results)

### Addition :relaxed:
If you need to convert existing txt files, you can use the following code:

```python
import pickle
import csv

data=[]
result = []


#Your path to the file from which to take data
in_file = open("/home/usr/result.txt", 'r')

for line in in_file:
    data.append(line.rstrip('\n'))
in_file.close()

#Your path to the file in which we will write the output of the converted data (p.s. if the file does not exist, it will be created).
in_other_file = open("/home/usr/result.csv", "w")
for ch in range(len(data)):
    data_in_data = data[ch].split('[')
    result.append(data_in_data)

write = csv.writer(in_other_file, quoting=csv.QUOTE_NONNUMERIC)
#Here you can specify the names of the columns according to the request that was executed (example according to point 1)
write.writerow(["Number of requests","IP"])

for row in result:
    write.writerow(row)
in_other_file.close() 
```

 :yellow_heart: :blue_heart:
