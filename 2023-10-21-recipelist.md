---
comments: True
layout: notebook
title: Recipe Search
description: 
type: hacks
courses: {'compsci': {'week': 1}}
categories: ['C4.1']
---

<html>
<head>
    <title>Recipe List</title>
    <div class="search-container">
    <input type="text" id="searchInput" placeholder="Search for recipes here">
    <button id="searchButton">Search</button>
</div>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f2f2f2;
            margin: 0;
            padding: 0;
        }
        .recipe-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            margin: 10px;
        }
        .recipe-card {
            flex: 0 0 calc(33.33% - 20px);
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 15px;
            padding: 10px;
            margin: 10px;
            background-color: white;
        }
        .recipe-card img {
            width: 100%;
            max-height: 200px;
            object-fit: cover;
        }
        .recipe-title {
            font-weight: bold;
            margin-top: 10px;
            color: black;
            font-size: 18px;
            font-family: Arial, sans-serif;
        }
        .more-info-button {
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            cursor: pointer;
            font-size: 16px;
            margin-top: 10px;
        }
        .more-info-button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <div id="recipeList" class="recipe-container">
        <!-- Content will be dynamically generated here -->
    </div>

    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const recipeList = document.getElementById("recipeList");
            const apiUrl = "https://backendrocketmain.stu.nighthawkcodingsociety.com/api/recipe/recipes";

            // Function to create the recipe details page
            function createRecipeDetailsPage(recipe) {
                // Redirect to the recipe details page when the button is clicked
                window.location.href = `https://deeskili.github.io/RocketSimFrontend/c4.1/2023/10/21/recipedetails.html?recipeId=${recipe.id}`;
            }
            // Function to find a matching image filename based on the recipe title
            function findMatchingImageFilename(recipeTitle) {
                const title = recipeTitle.toLowerCase();
                if (titleToImageMapping.hasOwnProperty(title)) {
                    return `images/Food%20Images/Food%20Images/${titleToImageMapping[title]}`;
                }
                return "default.jpg";
            }
            // Fetch data from the API for all recipes
            fetch(apiUrl)
                .then(response => {
                    if (!response.ok) {
                        throw new Error(`Network response was not ok: ${response.status}`);
                    }
                    return response.json();
                })
                .then(data => {
                    // Generate recipe cards for each recipe
                    data.forEach(recipe => {
                        const recipeCard = document.createElement("div");
                        recipeCard.classList.add("recipe-card");

                        // Display image (you may need to adjust this)
                        function findMatchingImageFilename(title) {
                            // Your implementation to find the image filename based on the recipe title
                            // Replace this with your actual logic
                            return 'path/to/your/image/' + title + '.jpg';
                         }

                        // Function to create and append the image to the recipe card
                        function displayRecipeImage(recipeCard, recipe) {
                            const imgElement = document.createElement("img");
                            imgElement.src = findMatchingImageFilename(recipe.title);
                            recipeCard.appendChild(imgElement);
                        }

                        // Display recipe title
                        const titleElement = document.createElement("div");
                        titleElement.classList.add("recipe-title");
                        titleElement.textContent = recipe.title;
                        recipeCard.appendChild(titleElement);

                        // More info button
                        const moreInfoButton = document.createElement("button");
                        moreInfoButton.classList.add("more-info-button");
                        moreInfoButton.textContent = "More Info";

                        // Add an event listener to the "More Info" button
                        moreInfoButton.addEventListener("click", () => {
                            createRecipeDetailsPage(recipe);
                        });

                        recipeCard.appendChild(moreInfoButton);

                        // Append the recipe card to the container
                        recipeList.appendChild(recipeCard);
                    });
                })
                .catch(error => {
                    console.error("There was a problem fetching the data:", error);
                });
        });

        // Mapping object for recipe titles to image filenames
        const titleToImageMapping = {
            "Miso-Butter Roast Chicken With Acorn Squash Panzanella": "miso-butter-roast-chicken-acorn-squash-panzanella.jpg",
            // Add more mappings as needed
        };
    </script>
</body>
</html>