# approach_explanation.md

## Approach Explanation Persona-Driven Document Intelligence

We have built this project to solve the problem of helping someone quickly find the most relevant parts of a set of documents, based on who they are (the persona) and what they need to do (the job-to-be-done). The goal is to build an offline tool that works fast, uses only CPU, and stays within a limited size, all while giving smart, useful results.

To achieve this, we found a solution that avoids large, slow models and focuses on a lightweight, fast, and practical approach. It uses a small sentence embedding model called `all-MiniLM-L6-v2`, which gives good results and loads quickly. For reading PDF files, we have used `pdfplumber`, which is simple and fast for extracting text.

The process starts by reading a file called `input.json` that contains the persona and their task. From that, it creates a few short search phrases to figure out what kind of content would be useful for that person.

Next, the system reads through up all the PDF documents, scanning all of the pages in each. For every page, it checks how similar the page’s content is to the persona's need, using sentence embeddings and a bit of keyword matching.

If a page looks relevant, the system tries to find a suitable title from the first few lines. It also pulls out a short summary by picking the most useful sentences on that page.

At the end, it picks the top 5 most relevant sections across all documents. It saves all the results, including document names, page numbers, section titles, summaries, and metadata,  all into an output file.

The whole solution is packaged inside a Docker container so it can be run easily on any system. It works completely offline, finishes in under 60 seconds, and follows all the rules given in the challenge.

This approach focuses on keeping things simple, fast, and effective, while still giving results that are actually helpful and meaningful for the user.

# Execution Instructions for Round 1B Dockerized Solution
=======================================================

1\. Prepare Your Input Directory
--------------------------------

1.  Create an input directory in your project root (same level as your Dockerfile and main.py).
    
2.  Inside the input directory, place:
    
    *   **The input.json file** with the following content (adapt values as needed):
        
    *   
        ```json 
            {
            "challenge_info": {
                "challenge_id": "round_1b_002",
                "test_case_name": "travel_planner",
                "description": "France Travel"
            },
            "documents": [
                {
                    "filename": "South of France - Cities.pdf",
                    "title": "South of France - Cities"
                },
                {
                    "filename": "South of France - Cuisine.pdf",
                    "title": "South of France - Cuisine"
                },
                {
                    "filename": "South of France - History.pdf",
                    "title": "South of France - History"
                },
                {
                    "filename": "South of France - Restaurants and Hotels.pdf",
                    "title": "South of France - Restaurants and Hotels"
                },
                {
                    "filename": "South of France - Things to Do.pdf",
                    "title": "South of France - Things to Do"
                },
                {
                    "filename": "South of France - Tips and Tricks.pdf",
                    "title": "South of France - Tips and Tricks"
                },
                {
                    "filename": "South of France - Traditions and Culture.pdf",
                    "title": "South of France - Traditions and Culture"
                }
            ],
            "persona": {
                "role": "Travel Planner"
            },
            "job_to_be_done": {
                "task": "Plan a trip of 4 days for a group of 10 college friends."
            }
        }
        ```
        
    *   Place **all** the mentioned PDF files inside the **same input directory**.
        
        *   For example:
            
            *   input/
                
                ├── input.json
                
                ├── South of France - Cities.pdf
                
                ├── South of France - Cuisine.pdf
                
                ├── South of France - History.pdf
                
                ├── South of France - Restaurants and Hotels.pdf
                
                ├── South of France - Things to Do.pdf
                
                ├── South of France - Tips and Tricks.pdf
                
                └── South of France - Traditions and Culture.pdf
                

> **Note:** Make sure filenames in input.json exactly match the PDF filenames placed in the input folder.

2\. Build Your Docker Image
---------------------------

From your project root (where Dockerfile is located), run:

```console
   docker build --platform linux/amd64 -t mysolutionname:somerandomidentifier .   
   ```

*   Replace mysolutionname:somerandomidentifier with your preferred image name/tag.
    
*   This will build the container image with all dependencies and pre-downloaded models.
    

3\. Run Your Docker Container for Processing
--------------------------------------------

Ensure you have an empty output directory ready:

```console
   mkdir -p output   
   ```

Then run:

```console   
    docker run --rm -v $(pwd)/input:/app/input -v $(pwd)/output:/app/output --network none mysolutionname:somerandomidentifier  

```

Explanation:

*   \--rm : removes the container after it finishes to avoid clutter.
    
*   \-v $(pwd)/input:/app/input : mounts your input folder.
    
*   \-v $(pwd)/output:/app/output : mounts the output folder.
    
*   \--network none : disables internet access (as required).
    
*   mysolutionname:somerandomidentifier : your built image name.
    

4\. Check Output
----------------

*   Once the container finishes, your output folder will contain output.json.
    
*   This JSON file contains the results of the round 1B analysis.
    

Summary Example Commands
------------------------

```console  


# Place input.json and all PDFs into input/  

# Build image  
docker build --platform linux/amd64 -t mysolutionname:somerandomidentifier .  

# Run container  
docker run --rm -v $(pwd)/input:/app/input -v $(pwd)/output:/app/output --network none mysolutionname:somerandomidentifier  

# View results  
cat output/output.json   
```

FAQs
====

Q: Where do I place my input.json and PDFs?
-------------------------------------------

A: Place both inside the local input directory. They will be available inside the container at /app/input.

Q: How does Docker access these files?
--------------------------------------

A: Docker volume mounting shares the host directory with the container's directory. So any files you place locally in input become visible inside the container at /app/input.

Q: Can the container write output files I can access?
-----------------------------------------------------

A: Yes! Files written inside container at /app/output appear in your local output directory automatically because of volume mounting.

If you follow these instructions exactly, your Docker image and container will correctly pick up the input files, process them, and save the output as expected.
