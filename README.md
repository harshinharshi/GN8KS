# Automation Script for Running and Documenting Prompts v2.3.1

This project aims to automate the process of running and documenting the results of various prompts and storing them into notebooks. 

# Gemini and GPT Notebook Generator

## Requirements

- Google Chrome version `125.0.6422.78` or higher
- A virtual environment with Python version 3.9 (You can also check `runtime.txt` for exact patch version I used)

## Setup

1. Ensure Google Chrome is installed and up-to-date.

2. Start two Chrome sessions in debug mode using one of the commands below depending on your Operating System:

- ### Start Chrome Session For Gemini

    **Windows:**
    ```sh
    "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir="C:\selenium_chrome_profile"
    ```

    **macOS:**
    ```sh
    /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --user-data-dir="/Users/<your-username>/selenium_chrome_profile"
    ```

    **Linux:**
    ```sh
    google-chrome --remote-debugging-port=9222 --user-data-dir="/home/<your-username>/selenium_chrome_profile"
    ```
- ### Start Chrome Session For ChatGPT

    **Windows:**
    ```sh
    "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9333 --user-data-dir="C:\selenium_chrome_profile_2"
    ```

    **macOS:**
    ```sh
    /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9333 --user-data-dir="/Users/<your-username>/selenium_chrome_profile_2"
    ```

    **Linux:**
    ```sh
    google-chrome --remote-debugging-port=9333 --user-data-dir="/home/<your-username>/selenium_chrome_profile_2"
    ```

3. Log into your Gemini account on the first chrome window.

4. Log into your ChatGPT account on the second chrome window.

5. For `Linux` and `Mac` users, you may get a permission error when trying to run the script because you need to explicitly give **execute permission/priviledge** to the chrome webdriver the script will try to use. To fix this, navigate to the project's `webdrivers` directory in terminal and give it that permission;
    ```bash
    cd webdrivers
    chmod +x chromedriver-[os_arc_type]
    ```
    - The script supports linux64, mac-arm64, mac-x64, win32 and win64 arc, and it will automatically chose the webdriver which is best suited for your OS. If you don't know the one to give permission to, you should be able to tell which webdriver the script tried using from the error message you will receive. Or you can just check from the list below.
        - `chromedriver-linux64`
        - `chromedriver-mac-arm64`
        - `chromedriver-mac-x64`
        - `chromedriver-win32.exe`
        - `chromedriver-win64.exe`

6. Ensure you have the libraries in `requirements.txt` installed into your python environment. For windows users, you may uncomment the`pywin32==306` and `pywinpty==2.0.13` library in the `requirements.txt` file before doing this.
    ```bash
    pip install -r requirements.txt
    ```

7. Open 2 termninal sessions, navigate to the root project directory where the scripts `gemini.py` and `chatgpt.py` are located and run them separately in the 2 terminals. 

8. From my personal experience, if your computer's screen is not big enough to have the 2 Chrome browser windows opened at maximum width side by side, you're better off executing the scripts one at a time so one brower can have enough space. This is so the 2 browsers don't overlap each other and mistakenly start clicking items in the other browser. You can first run `chatgpt.py` and wait for it finish executing before you do same for `gemini.py` or vice-versa. :)



**NOTE:** _Completely minimize mouse interactions to ensure a smooth process while the script is/are running as the script will mostly use the keyboard to type the file path when uploading files. If you're using the mouse elsewhere, the keyboard, will attempt to type the path of the file at wherever you focused the mouse instead of the web file input form popup. As it stands, both Gemini and GPT platforms don't make it possible to upload files using automated scripts, that's why I had to resort to the use of keyboard, in case you were wondering why. :)_

## Configuration

Create a `jobs.json` file in the script's directory with the structure below. You will be populating this file with your various prompts and file paths because these is where both `gemini.py` and `chatgpt.py` will be reading your inputs from:

**jobs.json**
```python
{
    "rater_id": "000", # Your unique rater id
    "tasks": [
        {
            "task_id": "100", # ID assigned to that row on google sheet
            # The script uploads all your files in the beginning of the chat.
            # So currently you won't be uploading different files per turn, all will be 
            # combined and uploaded at the very beginning of the chat session.
            "files": [
                {
                    # Relative path of file or dataset which you will be using for these prompts. 
                    "path": "relative_file_path_1", #Ex: "query_files/activities.csv"
                    # The google drive link to the file or dataset
                    "url": "https://url_of_file"
                },
                # ...
            ],
            "prompts": [
                "User Prompt 1",
                "User Prompt 2",
                
                # ...
            ]
        },
        {
            "task_id": "101", # ID assigned to that row on google sheet
            # The script uploads all your files in the beginning of the chat.
            # So currently you won't be uploading different files per turn, all will be 
            # combined and uploaded at the very beginning of the chat session.
            "files": [
                {
                    "path": "relative_file_path_1",
                    "url": "https://url_of_file"
                },
                {
                    "path": "relative_file_path_2",
                    "url": "https://url_of_file"
                }
                # ...
            ],
            "prompts": [
                "User Prompt 1",
                "User Prompt 2",
                "User Prompt 3",
                # ...
            ]
        }
    ]
}
```

- **rater_id**: The unique number assigned to the rater.
- **tasks**: A list of dictionaries, each representing a task.
  - **task_id**: The ID number given to that task on the excel sheet.
  - **files**: A list of file names relative to the script directory.
  - **prompts**: A list of prompt strings. The first prompt should be entered first, followed by the second, and so on.

There can be an infinite number of tasks.

## Usage

Once Chrome is running in debug mode and you are logged into Gemini:

1. Place your `jobs.json` file in the script directory.
2. Run the script to start the automation process.


#
# CLI Based Reproducibility Frequency out of 5

This also another automated script for running tasks/prompts through cli 5 times and storing all results into thier respective directories.

## Configuration

Create a `reproducible-jobs.json` file in the script's directory with the structure below. You will be populating this file with your various prompts your're trying to run 5 times. The `cbrfo5.py` script file will be reading your inputs that json file:

**reproducible-jobs.json**
```python
{
    "rater_id": "000", # Your unique rater id
    "tasks": [
        {
            "task_id": "100", # ID assigned to that row on google sheet
            # The script uploads all your files in the beginning of the chat.
            # So currently you won't be uploading different files per turn, all will be 
            # combined and uploaded at the very beginning of the chat session.
            "files": [
                "relative_file_path_1",
                # ...
            ],
            "prompts": [
                "User Prompt 1",
                "User Prompt 2",
                
                # ...
            ]
        },
        {
            "task_id": "101", # ID assigned to that row on google sheet
            # The script uploads all your files in the beginning of the chat.
            # So currently you won't be uploading different files per turn, all will be 
            # combined and uploaded at the very beginning of the chat session.
            "files": [
                "relative_file_path_1",
                "relative_file_path_2"
                # ...
            ],
            "prompts": [
                "User Prompt 1",
                "User Prompt 2",
                "User Prompt 3",
                # ...
            ]
        }
    ]
}
```

- **rater_id**: The unique number assigned to the rater.
- **tasks**: A list of dictionaries, each representing a task.
  - **task_id**: The ID number given to that task on the excel sheet.
  - **files**: A list of file names relative to the script directory.
  - **prompts**: A list of prompt strings. The first prompt should be entered first, followed by the second, and so on.

There can be an infinite number of tasks.

## Usage

Once you have populated the `reproducible-jobs.json` with your details:

1. First, add a `.env` file to the project's directory and provide the `API_KEY` and `MODEL` environment values, these will be required in other to make requests to the model's API.
- **.env**
    ```env
    API_KEY="add_api_key_here"

    MODEL="add_model_name_here"
    ```
2. Make sure your `reproducible-jobs.json` file is in the same directory as the `cbrfo5.py` file.
3. Run the cmd `python cbrfo5.py` to start the generating copies.
4. All various outputs generated will be located in a directory called `reproduced_outputs/ID_[task_id]`.

## Author

[Fred Dunyo](https://github.com/dunfred)

## Miscellaneous
1. Gemini Chrome Port: 9222
2. ChatGPT Chrome Port: 9333
