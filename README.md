import json

def clean_data(data):
    #Remove user with missing names 
    data["users"] = [user for user in data["users"] if user["name"].strip()]
    #Remove duplicate friends 
    for user in data["users"]:
        user['friends'] = list(set(user['friends']))
    #Remove inactive users
    data['users'] = [user for user in data["users"] if user['friends'] or user['liked_pages']]
    #Remove duplicate pages
    unique_pages = {} #Empty dictionary created
    for page in data['pages']:
        unique_pages[page['id']] = page
    data['pages'] = list(unique_pages.values())
        
    return data


data = json.load(open("garv.json")) #imported data
data = clean_data(data)
json.dump(data, open("cleaned_data.json","w"), indent =4) 
print("Data cleaned sucessfully")
