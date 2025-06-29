Do you frequently check if Alek has released a new blog post? 
Well, if you want you can instead run the following script. 

Or better yet, you can setup a `cron` job to periodically run this script every hour, and when the output is something new, it pings you.

Thanks to Sonnet3.7 for zero-shot generating this script.

```python
import requests
from bs4 import BeautifulSoup
from datetime import datetime, timedelta

def check_recent_posts(url, days=3):
    # Get the current date
    current_date = datetime.now()
    
    # Calculate the cutoff date (3 days ago)
    cutoff_date = current_date - timedelta(days=days)
    
    # Fetch the webpage
    response = requests.get(url)
    
    # Check if the request was successful
    if response.status_code != 200:
        print(f"Failed to retrieve the webpage: {response.status_code}")
        return []
    
    # Parse the HTML content
    soup = BeautifulSoup(response.content, 'html.parser')
    
    # Find all blog post elements - based on the provided HTML structure
    blog_posts = soup.select("li.section-li")
    
    recent_posts = []
    
    for post in blog_posts:
        # Find the date element
        date_element = post.select_one("p.meta")
        
        if date_element:
            date_text = date_element.text.strip()
            
            try:
                # Parse the date (format seems to be "MMM DD, YYYY")
                post_date = datetime.strptime(date_text, '%b %d, %Y')
                
                # Check if the post is recent
                if post_date >= cutoff_date:
                    # Get the title
                    title_element = post.select_one("h3 a")
                    title = title_element.text.strip() if title_element else "Unknown Title"
                    
                    # Get the link
                    link = title_element['href'] if title_element else None
                    
                    # Add to recent posts
                    recent_posts.append({
                        'title': title,
                        'date': post_date.strftime('%Y-%m-%d'),
                        'link': link
                    })
            except ValueError:
                print(f"Could not parse date: {date_text}")
                continue
    
    return recent_posts

def main():
    url = "https://awestover.github.io/thoughts/0stack/"
    days_to_check = 3
    
    print(f"Checking for posts in the last {days_to_check} days...")
    recent_posts = check_recent_posts(url, days_to_check)
    
    if recent_posts:
        print(f"Found {len(recent_posts)} recent post(s):")
        for post in recent_posts:
            print(f"- {post['title']} ({post['date']})")
            if post['link']:
                print(f"  Link: {url.rstrip('/')}{post['link']}")
    else:
        print("No recent posts found in the specified time period.")

if __name__ == "__main__":
    main()

```


Anthony has a nicer way of doing this: he gets email notifications. I'm not sure how he does it.
Anthony maybe you should send me a description of how to setup email notifications.
Here's Claude's two cents.

```
Yes, you can set up email notifications when someone (like Alek) pushes changes to a GitHub repository. Let me explain how you can do this:

## Using GitHub Watch Feature

1. Navigate to Alek's repository (https://github.com/awestover/thoughts)
2. Click the "Watch" button at the top right of the repository
3. Select "Custom" from the dropdown menu
4. Check "Releases", "Discussions", "Security alerts" and any other notifications you want
5. Make sure your GitHub notification settings are configured to send emails

## Setting up GitHub Notification Emails

1. Go to your GitHub account settings
2. Navigate to "Notifications"
3. Make sure "Email notifications" is enabled
4. Choose your notification preferences (e.g., receive emails when watching repositories)

## Using Third-Party Services

For more specific notifications (like only when certain files change), you could use a service like:

- Zapier - Connect GitHub to your email service
- IFTTT - Create applets that trigger emails when GitHub events happen
- GitNotifier - A dedicated service for GitHub notifications

## Creating a Custom Solution

If you're technically inclined, you could also:

1. Set up a GitHub webhook on the repository
2. Create a simple server to receive the webhook events
3. Process the events and send yourself an email when changes are pushed

Would you like more detailed instructions on any of these approaches?
```
