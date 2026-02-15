# Freq. Finder Setup and Build Instructions

Version: 260215.1200

This guide provides instructions for setting up the development environment and building the application executable.
<!-- ... (skipping unchanged sections) ... -->
                <li><strong>Interference Analysis:</strong> Overrides standard frequency filters. Input a target frequency (in MHz) to find:
                    <ul>
                        <li><strong>Adjacent Channels:</strong> Neighbors +/- 12.5 kHz.</li>
                        <li><strong>Causing Interference:</strong> Licenses listening at your harmonic frequencies (x2, x3, x4).</li>
                        <li><strong>Receiving Interference:</strong> Licenses transmitting at your sub-harmonic frequencies (/2, /3, /4).</li>
                    </ul>
                    Includes a diagnostic display of all scanned frequencies and highlights "Prime Suspects" (exact harmonic matches, +/- 2.5 kHz) in <strong>bold red</strong> in the results. A geographic filter is required.</li>
            </ul>

            <h3>Cellular Search</h3>



## Environment Setup

It is highly recommended to use a Python virtual environment to manage project dependencies. These instructions assume you have a compatible version of Python installed and available in your system's PATH.

### 1. Navigate to Project Directory

Open your terminal or command prompt and change to the project's root directory:

```sh
cd path\to\Freq.Finder
```

### 2. Create and Activate the Virtual Environment

**On Windows:**

```powershell
# Create the virtual environment
python -m venv venv

# Activate it
.\venv\Scripts\activate
```

**On macOS / Linux:**

```sh
# Create the virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate
```

After activation, your command prompt should be prefixed with `(venv)`.

### 3. Install Dependencies

With the virtual environment activated, install the required packages from the updated `requirements.txt`:

```sh
pip install -r requirements.txt
```

## Docker

This project can be run as a Docker container. This is the recommended way to run the application in production.

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) must be installed and running on your system.

### Building the Docker Image

To build a local Docker image, run the following command from the project's root directory:

```sh
docker build -t freq-finder .
```

### Running the Docker Container

To run the application, pull the image from the GitHub Container Registry (`ghcr.io`) and run it. The following command will automatically download the correct image for your system's architecture (e.g., Intel/AMD or Apple Silicon).


```sh
# Replace `YOUR_GITHUB_USERNAME` with the GitHub username where the package is hosted.
# Replace `/path/to/your/uls_data.duckdb` with the local path to your database file.
#docker run -p 7223:5000 -v "/path/to/your/uls_data.duckdb:/app/uls_data.duckdb" ghcr.io/YOUR_GITHUB_USERNAME/freq-finder:latest
# above is for later - store ULS db in a Docker volume for persistance. For now, it's OK within the container itself (below).
docker run -p 7223:5000 ghcr.io/paceplandotus/freq-finder:latest
```

This command does the following:

- `-p 7223:5000`: Maps port 7223 on your local machine to port 5000 inside the container.
- `-v "/Users/[yourname]]/Repos/Freq.Finder/uls_data.duckdb:/app/uls_data.duckdb"`: Mounts the local `uls_data.duckdb` file into the container. This allows the data to persist even if the container is removed.
- `freq-finder`: The name of the image to run.

Once the container is running, you can access the application by opening a web browser and navigating to `http://localhost:7223`.

### Resource Allocation

The data maintenance process can be memory-intensive. If the container crashes during the database build, you may need to increase the memory allocated to Docker Desktop.

**On macOS:**

1.  Open Docker Desktop.
2.  Go to **Settings > Resources > Advanced**.
3.  Increase the **Memory** allocation (e.g., to 8GB).
4.  Click **Apply & Restart**.

If you want to share the application as a single file, you can save the Docker image as a `.tar` file.

**1. Save the image to a file:**

```sh
docker save -o freq-finder.tar freq-finder
```

This will create a `freq-finder.tar` file in your current directory.

**2. Share the `.tar` file:**

You can then share this `freq-finder.tar` file with the other person.

**3. Load the image from the `.tar` file:**

The other person can then load the image into their Docker environment using the following command:

```sh
docker load -i freq-finder.tar
```

After they run this command, the `freq-finder` image will be available in their local Docker image registry, and they can run it using the `docker run` command in this README.

## Desktop Application (PyInstaller)

You can package Freq. Finder as a standalone desktop application using PyInstaller. This is ideal for offline use or for users who do not have Python installed.

### 1. Install PyInstaller

Ensure you have PyInstaller installed in your virtual environment:

```sh
pip install pyinstaller
```

### 2. Build the Executable

Run the build process using the provided specification file:

```sh
pyinstaller freq.finder.spec
```

### 3. Run the Application

After the build finishes, the standalone application will be located in the `dist/FreqFinder` folder.

- **Windows:** Run `dist/FreqFinder/FreqFinder.exe`.
- **macOS/Linux:** Run `dist/FreqFinder/FreqFinder`.

The application will launch a console window showing the server status. Open your web browser and go to `http://127.0.0.1:7223`.