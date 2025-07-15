import requests
import json
import time
ACCESS_TOKEN = input("nhập token:")
COOKIE = input("nhập cookie:")
POST_ID =  input("nhập POST_ID:")

#COOKIE= ''''''
#POST_ID = "106374731851554_705973955279492"  # Có thể cần PAGE_ID_1173143444842789
headers = {
    'Accept': 'application/json',
    'Accept-Language': 'vi-VN,vi;q=0.9,fr-FR;q=0.8,fr;q=0.7,en-US;q=0.6,en;q=0.5',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36',
    'Sec-Fetch-Dest': 'empty',
    'Sec-Fetch-Mode': 'cors',
    'Sec-Fetch-Site': 'same-origin',
    'cookie': COOKIE,
}

BASE_URL = "https://graph.facebook.com/v20.0"

# Initialize variables
all_comments = []
cursor = None
limit = 100  # Số bình luận mỗi trang (tối đa 100)  ĐÂY LÀ MỖI 1 LẦN QUÉT THÌ LẤY 100 CMT
max_attempts = 100  # Giới hạn số lần gọi API | ĐÂY LÀ SỐ LẦN QUÉT CMT 


attempt = 0
while attempt < max_attempts:

    params = {
        "access_token": ACCESS_TOKEN,
        "fields": "id,message,created_time",
        "limit": limit,
    }
    if cursor:
        params["after"] = cursor
    
    url = f"{BASE_URL}/{POST_ID}/comments"

    try:
        response = requests.get(url, params=params,  headers=headers).json()
        time.sleep(2)  
    except requests.exceptions.RequestException as e:
        print(f"Error making API request: {e}")
        break

    try:
        comments = response.get("data", [])
        paging = response.get("paging", {})
        next_cursor = paging.get("cursors", {}).get("after")
        comment_count = len(comments)
        print(f"Attempt {attempt + 1}: Cursor = {cursor}, Comments fetched = {len(comments)}, Has next page = {bool(next_cursor)}")
        all_comments.extend(comments)
        print(f"Fetched {len(all_comments)} comments")
        

        if not next_cursor or not comments:
            break
        
        cursor = next_cursor
    except KeyError as e:
        print(f"Error accessing response data: {e}")
        print("Response:", json.dumps(response, indent=2))
        break
    
    attempt += 1

print(f"\nTotal number of comments fetched: {len(all_comments)}")
for i, comment in enumerate(all_comments, 0):
    try:
        comment_text = comment.get("message", "[No text]")
        comment_id = comment.get("id", "Unknown")
        print(f"Comment {i}: {comment_text} (ID: {comment_id})")
    except KeyError:
        print(f"Comment {i}: [Error: No text available]")
