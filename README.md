# capstone
midterm project source code

import java.util.ArrayList;
import java.util.Scanner;
class Main {
  	private static Scanner sc = new Scanner(System.in);
    public static final String TEXT_RESET = "\u001B[0m";
    public static final String TEXT_BLACK = "\u001B[30m";
    public static final String TEXT_RED = "\u001B[31m";
    public static final String TEXT_GREEN = "\u001B[32m";
    public static final String TEXT_YELLOW = "\u001B[33m";
    public static final String TEXT_BLUE = "\u001B[34m";
    public static final String TEXT_PURPLE = "\u001B[35m";
    public static final String TEXT_CYAN = "\u001B[36m";
    public static final String TEXT_WHITE = "\u001B[37m";

    public static final String ANSI_BLACK_BACKGROUND = "\u001B[40m";
    public static final String ANSI_RED_BACKGROUND = "\u001B[41m";
    public static final String ANSI_GREEN_BACKGROUND = "\u001B[42m";
    public static final String ANSI_YELLOW_BACKGROUND = "\u001B[43m";
    public static final String ANSI_BLUE_BACKGROUND = "\u001B[44m";
    public static final String ANSI_PURPLE_BACKGROUND = "\u001B[45m";
    public static final String ANSI_CYAN_BACKGROUND = "\u001B[46m";
    public static final String ANSI_WHITE_BACKGROUND = "\u001B[47m";
    
    public static void main(String[] args) {

      BookSystem library = new BookSystem();
		  addLibrary(library);
		 
		 
	     launch(library);
	     sc.close();
    }

	 public static void addLibrary(BookSystem library) {
		 library.addBook(new Book("Aşk", "Elif Şafak" , "Fiction"));
		 library.addBook(new Book("Java Software Solutions", "Lewis / Loftus/ Cocking", "Nonfiction"));
		 library.addBook(new Book( "A Guide to Programming Java", "Brown", "Nonfiction"));
		 library.addBook(new Book("Ender's Game", "Orson Scott Card",  "Science Fiction"));
		 library.addBook( new Book("Harry Potter and the Sorcerer's Stone","J.K. Rowling", "Fantasy"));
	 }
	 
	 public static void addNewBook(BookSystem library) {
		 
		 System.out.println("Enter the name of the book");
		 String title = sc.nextLine();
		 System.out.println("Enter author of the book");
		 String author = sc.nextLine();
		 System.out.println("Enter genre of the book");
		 String genre = sc.nextLine();
		 Book newBook = new Book(title, author, genre);
		 boolean isExist = library.addBook(newBook);
		 if(!isExist) {
			 System.out.println("New book is in the library list now!");
			 System.out.println();
		 }
		 
	 }
	 public static void removeBook(BookSystem library) {
		 System.out.println("Enter the name of the book");
		 String title = sc.nextLine();
		 library.removeBook(title);
		 
	 }
	 
		public static void launch(BookSystem library) {
	
			  while(true) {
				  System.out.println(ANSI_YELLOW_BACKGROUND + "Choices(Press any other character to quit):\n1.List books\n2.Checkin book\n3.Checkout Book\n4.Search a Book\n5.Add a new Book\n6.Remove a Book from Library" + TEXT_RESET);
				  int option = sc.nextInt();
				  sc.nextLine();
				  switch(option) {
				  case 1:
					  System.out.println();
					  printLineBreak();
					  
					  System.out.println(library.listBooks());
					  break;
				  case 2:
					  System.out.println();
					  System.out.println("Which book do you want to checkin?");
					  String title = sc.nextLine();
					  boolean isCheck = library.checkInBook(title);
					  if(isCheck) {
						  System.out.println(ANSI_CYAN_BACKGROUND+ "Checkin completed" + TEXT_RESET);
						  System.out.println();
					  }else {
						System.out.println(ANSI_CYAN_BACKGROUND+"No this book in the library" + TEXT_RESET);
						System.out.println();
					  }
					  break;
				  case 3:
					  System.out.println();
					  System.out.println("Which book do you want to checkout?");
					  String btitle = sc.nextLine();
					  boolean isCheckOut = library.checkOutBook(btitle);
					  System.out.println();
					  break;
				  case 4:
					  System.out.println("Enter the title of the book you want to search");
					  String otitle = sc.nextLine();
					  Book oBook =   library.searchBook(otitle);
					  if(oBook!=null) {
						  System.out.println(oBook.toString());
						  System.out.println();
					  }else {
						  System.out.println(ANSI_CYAN_BACKGROUND + "No this book in the library" + TEXT_RESET);
						  System.out.println();
					  }
					  break;
				  case 5:
					  addNewBook(library);
					  break;
				  case 6:
					  removeBook(library);
					  break;
					  default:
						  System.out.println("Invalid choice!!!");
						  break;
						  
					  
				  }
			  }
		}
		
		public static void printLineBreak() {
		System.out.printf(TEXT_RED +"%-50s%-30s%-20s%-15s%-10s%n","Title", "Author", "Genre","Is Available","ID" + TEXT_RESET);
		System.out.println("----------------------------------------------------------------------------------------------------------------------");
		}

}

book.java

public class Book {
	private int id;
	private String title;
    private String author;
    private String genre;
    private boolean isCheckIn;
    private static int nextId=0;

	public Book(String title, String author, String genre) {
		this.id = nextId;
		this.title = title.toUpperCase();
		this.author = author.toUpperCase();
		this.genre = genre.toUpperCase();
		isCheckIn = true;
		nextId++;
	}
	
	public int getId() {
		return id;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getAuthor() {
		return author;
	}

	public void setAuthor(String author) {
		this.author = author;
	}
	
    public String getGenre(){
        return genre ;
    }
    
    public void setGenre(String genre){
        this.genre = genre;
    }
    
    public boolean getCheckIn() {
    	return isCheckIn;
    }
    public void setCheckOut() {
    	isCheckIn = false;
    }
    public void setCheckIn() {
    	isCheckIn = true;
    }
	
	public String toString(){
		return String.format("%-50s%-30s%-20s%-15s%-10s%n", title, author, genre, isCheckIn,id);
	}

}


BookSystem.java

import java.util.ArrayList;

public class BookSystem {
	private ArrayList<Book> books;

  public static final String ANSI_CYAN_BACKGROUND = "\u001B[46m";
  public static final String TEXT_RESET = "\u001B[0m";
	
	public BookSystem() {
		books = new ArrayList<Book>();
	}
	
	public boolean addBook(Book b) {
		boolean isBookExist = false;
		for(Book arrBook:books) {
			if(arrBook.getTitle().equalsIgnoreCase(b.getTitle()) && arrBook.getAuthor().equalsIgnoreCase(b.getAuthor())) {
				isBookExist = true;
				System.out.println(ANSI_CYAN_BACKGROUND + "The book is already exist in the library!" + TEXT_RESET);
			}
		}
		if(!isBookExist) {
			books.add(b);
		}
		return isBookExist;
	}
	
	public void removeBook(String title) {
		for(int i=0;i<books.size();i++) {
			if(books.get(i).getTitle().equalsIgnoreCase(title)) {
				books.remove(i);
				System.out.println(ANSI_CYAN_BACKGROUND + "Removing book completed!!" + TEXT_RESET);
        System.out.println();
			}
		}
	}
	
	public Book searchBook(String name) {
		for(Book arrBook:books) {
			String title = arrBook.getTitle();
			if(title.equalsIgnoreCase(name)) {
				return arrBook;
			}
		}
		return null;
	}
	
	public String listBooks() {
		String listBooks = "";
		for(Book b:books) {
			listBooks += b.toString();
		}
		return listBooks;
	}
	
	public boolean checkInBook(String name) {
		for(int i=0;i<books.size();i++) {
			if(books.get(i).getTitle().equalsIgnoreCase(name)) {
				  books.get(i).setCheckIn();
				  return true;
			}
		}
		return false;
	}
	
	public boolean checkOutBook(String name) {
		for(int i=0;i<books.size();i++) {
			if(books.get(i).getTitle().equalsIgnoreCase(name)) {
				if(books.get(i).getCheckIn()) {
					books.get(i).setCheckOut();
					System.out.println(ANSI_CYAN_BACKGROUND +"Checkout completed! Good Reading.." + TEXT_RESET);
					return true;
				}else {
					System.out.println(ANSI_CYAN_BACKGROUND +"The book is already checked out" + TEXT_RESET);
					return false;
				}
			}
		}
		System.out.println("No this book in the library..");
		return false;
	}

}
