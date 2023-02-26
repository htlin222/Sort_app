# README

- move this app to any folder you want to sort files
- double click and run
- Edit `./Contents/Resources/script` PRN

```sh
# Get the current working directory
current_dir=$(pwd)

# Get the parent directory
parent_dir=$(dirname "$current_dir")

# Get the grandparent directory
grandparent_dir=$(dirname "$parent_dir")

watch_path=$(dirname "$grandparent_dir")

# Loop through all the files in the directory
for file in "$watch_path"/*; do
    # Ignore directories, files that start with a dot, and files that end with .app
    if [[ -f "$file" && "${file##*/}" != .* && "${file##*.}" != "app" ]]; then
        # Get the extension of the file in uppercase
        if [[ "${file##*.}" == "zip" ]]; then
            extension="ZIP"
        else
            extension=$(echo "${file##*.}" | tr '[:lower:]' '[:upper:]')
        fi
        echo "Extension: $extension"

        # Get the creation date of the file
        creation_date=$(stat -f "%Sm" -t "%Y-%m-%d" "$file")

        # Move the file to the appropriate folder
		# edit the folder name here 🌟
        mkdir -p "$watch_path/🗄️$extension/$creation_date"
        dest_path=$(realpath "$watch_path/🗄️$extension/$creation_date")
        mv "$file" "$dest_path/${file##*/}" # handle spaces and special characters in filename
        echo "Moved $file to $dest_path/${file##*/}"
    fi
done

# Show a popup message with "Done" and "OK" button
e
```



