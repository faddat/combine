This is a bash script that I use to make it easier to work with LLMs.

I keep mine in

~/go/bin/combine

but you could keep it anywhere.


```bash
#!/bin/bash

# Prompt the user to enter the folder path
read -p "Enter the folder path containing the code files: " folder

# Specify the output file name
output_file="combined_code.txt"

# Remove the output file if it exists
rm -f "$output_file"

# Add a message for the LLM at the beginning of the file
echo "This file contains the combined code from multiple files in a folder. The purpose is to provide the LLM with a single, consolidated file for analysis and understanding of the entire codebase." >> "$output_file"
echo -e "// ===========================\n" >> "$output_file"

# Find all code files in the folder and subfolders, excluding the node_modules folder
find "$folder" -type f \( -name "*.go" -o -name "*.py" -o -name "*.js" -o -name "*.rs" \) -not -path "*/node_modules/*" | while read file; do
    # Add the filename as a comment
    echo "// File: $file" >> "$output_file"
    
    # Add the file content
    cat "$file" >> "$output_file"
    
    # Add a separator between files
    echo -e "\n// ===========================\n" >> "$output_file"
done
```
