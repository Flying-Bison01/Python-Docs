import requests
from bs4 import BeautifulSoup
import pandas as pd

# Initialize a list to store questions and images
data = []

# Loop through the pages (example: pages 1 to 5)
for i in range(1, 113):
    url = f"https://free-braindumps.com/microsoft/free-az-900-braindumps.html?p={i}"
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    
    # Find all paragraphs with the class 'lead'
    lead_paragraphs = soup.find_all('p', class_='lead')
    
    # Loop through each 'lead' paragraph and extract text and image
    for lead_paragraph in lead_paragraphs:
        text = lead_paragraph.get_text(separator="\n", strip=True)
        image_tag = lead_paragraph.find('img')
        image = image_tag['src'] if image_tag else 'No image'
        
        # Append the extracted data to the list
        data.append([text, image])
        print("Text:", text)
        print("Image URL:", image)
        print("\n" + "-"*50 + "\n")

# Convert the list to a DataFrame
df = pd.DataFrame(data, columns=['Question', 'Image URL'])

# Save the DataFrame to a CSV file
df.to_csv('index.csv', index=False)

# Optional: Print the DataFrame to check the results
print(df)
