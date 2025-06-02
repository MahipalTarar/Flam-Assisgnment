 ##Book Review Android App 

An MVVM Android application to read and review books with mock data. 
Users can see book details, add them to favorites, and see favorites even when offline using Room Database.

##Problem Statement
Create an Android application in which users can:

- See a list of books retrieved from a mock API.
- See detailed details of each book.
- Mark/unmark books as favorites.
- Cache favorite books locally to enable offline access.

## My Solution of given problem

In order to address the said problem, we created a neatly organized Android application with **MVVM architecture**:
 **Model** → Manages data (API + Room DB).
 **ViewModel** → Controls UI-related data and business logic.
 **View** → Presents the data to the user.

### Key Features

| Feature                 | Description                                                |
|-------------------     |------------------------------------------------------------|
|  Book Listing          | Displays all books in a scrollable list.                      |
|  Book Details          | View full book details such as author, rating, description. |
| dd to Favorites        | Favorite books marked with single click.                   |
|  Offline Support        | Favorites stored in local db through Room DB.            |

## Architecture - MVVM Pattern
com.bookreviewapp
│
├── model # Data Layer (Book, DAO, Database, API)
├── repository # Combines DB + API calls
├── viewmodel # LiveData + Business logic
├── view # UI Layer (Activities + XML layouts)
└── utils # Helpers, mock data loading




My Solution: A Structured and Efficient MVVM Approach
To tackle these challenges, I'm implemented a well-structured Android application following the industry-standard MVVM (Model-View-ViewModel) 
architectural pattern. This pattern promotes a clear separation of concerns, making the app easier to develop, maintain, and test. Here's how we broke it down:

The Model (Data Brains): This layer is the heart of our data operations. It defines how book data is structured, manages local storage using a Room database 
(think of it as a smart, local filing cabinet for your favorites), and handles the fetching of book data from a simulated API using Retrofit.
The ViewModel (Logic Hub): Acting as the intelligent intermediary, the ViewModel is responsible for preparing and managing data for the user interface. 
It acts as a bridge between the raw data and what the user sees, ensuring that the UI always displays the most up-to-date information.
The View (User Interface): This is what the user directly interacts with – the screens, buttons, and text that make up the app. 
It's designed to be responsive and visually appealing, displaying the book lists and detailed information.
By adopting this architecture, we achieved a highly maintainable and scalable solution, ensuring that adding new features or making changes wouldn't disrupt the entire application.

Model Layer:

Book.java: The blueprint for a book, containing properties like title, author, description, and image URL.
BookDao.java: Our interaction point with the Room database, defining how we insert, delete, and retrieve favorite books.
BookDatabase.java: The central access point for our local database.
BookApiService.java: The interface used by Retrofit to fetch book data from our (mocked) API.
ViewModel Layer:

BookViewModel.java: This class holds the LiveData objects that the UI observes. It delegates tasks like fetching books and adding favorites to the Repository,
ensuring the UI remains responsive and separate from data logic.
Repository Layer:

BookRepository.java: This is our "single source of truth" for data. It intelligently decides whether to fetch data from the API or retrieve it from the local Room database, simplifying the ViewModel's responsibilities.
View Layer:

MainActivity.java: The main screen that displays the scrollable list of books using a RecyclerView. Tapping a book navigates to its detailed view.
BookDetailActivity.java: This screen presents all the details of a selected book and includes a prominent "Favorite" button to save it locally.
Important Implementation Snippets
To illustrate the core functionality:

BookDao.java (Room Database Operations):

Java

@Insert
void insert(Book book);

@Delete
void delete(Book book);

@Query("SELECT * FROM Book")
LiveData<List<Book>> getAllFavorites();
These simple annotations allow Room to handle the complex database interactions for us.

BookRepository.java (Combining Data Sources):

Java

public LiveData<List<Book>> getBooks() { /* Logic to fetch from API */ }
public void addToFavorites(Book book) { /* Logic to insert into Room DB */ }
The repository seamlessly orchestrates fetching books from the API and saving favorites to the local database.

MainActivity.java (Observing LiveData):

Java

viewModel.getBooks().observe(this, adapter::submitList);
This line elegantly connects our UI to the LiveData in the ViewModel, automatically updating the book list whenever new data is available.

BookDetailActivity.java (Adding to Favorites):

Java

favBtn.setOnClickListener(v -> viewModel.addFavorite(book));
A simple click triggers the ViewModel to add the current book to the user's favorites.

The Power of Offline Support with Room DB
One of the standout features of this app is its robust offline capability for favorite books, powered by the Room persistence library. This means:

Persistent Favorites: Users can close the app, restart their device, and their favorite books will still be there.
No Internet Required: Once a book is favorited, viewing it doesn't require an active internet connection. Room handles all the local storage magic behind the scenes.
This is achieved efficiently with a background executor:

Java

Executors.newSingleThreadExecutor().execute(() -> bookDao.insert(book));
This ensures database operations don't block the main UI thread, keeping the app smooth and responsive.

Essential Tools and Libraries
We leveraged a suite of powerful Android development tools and libraries:

MainActivity: Features a RecyclerView to present the list of books in a scrollable format.
BookDetailActivity: Contains TextViews to display detailed information about a selected book and a Button to add it to favorites.
Important Development Considerations
To pass Book objects between activities efficiently, we made the Book class Parcelable.
The API URL for fetching books currently points to a mocked books.json file, demonstrating the app's functionality without requiring a live backend.
In Simple Words: Your Personalized Reading Companion
In essence, we've built a straightforward yet powerful Android application where users can effortlessly browse a collection of books, delve into their details, and mark their beloved reads as favorites. The beauty of it all is that these cherished favorites remain accessible even when an internet connection isn't available. By meticulously following the MVVM architecture and harnessing the power of Room for local data storage and Retrofit for data retrieval, we've created a clean, efficient, and user-friendly book review experience.

