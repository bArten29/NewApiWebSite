app = Flask(__name__)

@app.route("/", methods=["GET", "POST"])
def home():
    # Default API request URL
    url = "https://newsapi.org/v2/top-headlines?country=us&category=technology&apiKey=599f575770d34b888b26731f9061e967"

    if request.method == "POST" and "sort" in request.form:
        # If the form was submitted and the "sort" button was clicked, get the selected country and category values
        country = request.form["country"]
        category = request.form["category"]

        # Construct a new API request URL with the sorted data
        url = f"https://newsapi.org/v2/top-headlines?country={country}&category={category}&apiKey=599f575770d34b888b26731f9061e967"

    # Make API request and extract relevant data
    response = requests.get(url)
    data = response.json()
    articles = []
    for article in data["articles"]:
        title = article["title"]
        description = article["description"]
        url = article["url"]
        articles.append({"title": title, "description": description, "url": url})

    # Sort articles by title if "sort" button was clicked
    if request.method == "POST" and "sort" in request.form:
        articles.sort(key=lambda x: x["title"])

    # Render HTML template with news data
    return render_template("index.html", articles=articles)

if __name__ == "__main__":
    app.run(debug=True)
