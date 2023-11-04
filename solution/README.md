# csv server task

## Pull images

```bash
docker pull infracloudio/csvserver:latest
docker pull prom/prometheus:v2.22.0
```

## Run container

```bash
docker run -d --name csvserver -p 9393:9300 infracloudio/csvserver:latest
```

```Note:``` Its giving error that /csvserver/inputdata file is not exists, so need to create file with below script and than add as volume

## create bash script ```gencsv.sh```

```bash
#!/bin/bash

if [ "$#" -ne 2 ]; then
  echo "Usage: $0 <start_index> <end_index>"
  exit 1
fi

start_index=$1
end_index=$2

if [ "$start_index" -ge "$end_index" ]; then
  echo "Start index must be less than end index."
  exit 1
fi

output_file="inputdata"

# Remove the output file if it exists
if [ -e "$output_file" ]; then
  rm "$output_file"
fi

for ((i = start_index; i <= end_index; i++)); do
  random_number=$((RANDOM % 1000))
  echo "$i, $random_number" >> "$output_file"
done

echo "CSV file $output_file generated with entries from $start_index to $end_index."
```

## Run the script

```bash
chmod +x gencsv.sh
sh gencsv.sh 2 8
```

## Run container

```bash
## Remove old container then run below command
docker run -d --name csvserver -p 9393:9300 -v "$(pwd)/inputFile:/csvserver/inputdata" infracloudio/csvserver:latest
```

## Run Container with environment variable ```CSVSERVER_BORDER=Orange```

```bash
docker run -d --name csvserver -e CSVSERVER_BORDER=Orange -p 9393:9300 -v "$(pwd)/inputFile:/csvserver/inputdata" infracloudio/csvserver:latest
```

## Check output

![image](https://github.com/Naresh240/csvserver-task/assets/58024415/3e9da16e-ea47-4536-b828-6b8dc16d7ca5)
