# Web-Services
Build a REST-based XML(JSON) WCF web service and a Client to consume the Web Service
* IDE: Visual Studio
* Programming Language: c#
* Framework: WCF Application

#1. REST-based `XML` WCF Web Service
##1) Web Service Interface
```go
[ServiceContract]// interface xxx's attribute, indicating that interface xxx defines a service contract in WCF Application
public interface xxx
   {
        // *add an entry to the xx database
        [OperationContract] //method1's attribute, indicating that method1 is an operation which is part of the service contract in WCF Application
        [WebGet(UriTemplate = "/method1/{parameter1}/{parameter2}/{parameter3}")] //method1's attribute, indicating method1 is a retrieval opeartion, can be called by the WCF REST model.
        void method1(string parameter1, string parameter2, string parameter3);

        // *retrieve xx entries with a given property
        [OperationContract]
        [WebGet(UriTemplate = "/method2/{parameter1}")]
        PhoneBookEntry[] method2(string parameter1);
   } // end interface xxx
```
##2) Web Service Implementation
```go
public class xxxWebService : xxx
    {
        // Create a REST-base web service that stores entries in a database xxx.mdf 
        // and client that consume this service.
        // create a dbcontext object to access xxx database
        private xxxEntities1 dbcontext = new xxxEntities1();

        // add an entry to the xxx database
        public void method1(string parameter1, string parameter2, string parameter2)
        {
            // *create PhoneBook entry to be inserted in database
            PhoneBook content = new PhoneBook();
            content.LastName = lastName;
            content.FirstName = firstName;
            content.PhoneNumber = phoneNumber;

            // *insert PhoneBook entry in database
            dbcontext.PhoneBooks.Add(content);
            //dbcontext.Entry(phoneBookEntry).State = System.Data.EntityState.Added;
            dbcontext.SaveChanges();

        } // end method1

        // retrieve entries with a given parameter1
        // The method2 should return an array of strings
        public PhoneBookEntry[] method2(string parameter1)
        {
            //*retrieve entries from the database according to parameter1 via LINQ
            var selectedByLastName = from phonebook in dbcontext.PhoneBooks
                                     where phonebook.LastName == lastName
                                     select phonebook;

            PhoneBookEntry[] phoneBookList = new PhoneBookEntry[selectedByLastName.Count()];

            //return an array of strings.
            int i = 0;
            foreach (var phonebook in selectedByLastName)
            {

                phoneBookList[i++] = new PhoneBookEntry(phonebook.FirstName, phonebook.LastName, phonebook.PhoneNumber);

            }

            return phoneBookList;
        }//end method2
    }
```
##3) Deploying a REST-based XML WCF web service
* Open `xxxWebService.svc` in browser but don't close it while a client consuming the web service.

##4) Create a `Client` to consume the REST-based XML WCF web service - using HttpClient and getStringAsync
```go
  private HttpClient client = new HttpClient();      // *create an object to invoke to PhoneBookService
  client.GetAsync(new Uri(service/method/parameter)); //method GetAsync(send a GET request to the specified URI as asynchronous operation)
  client.GetStringAsync(new Uri(service/method/parameter)); //method GetStringAsync(send a GET request to the specified URI and return the response body as a string)
```

#2. REST-based `JSON` WCF web service
##1) Web Service Interface(one place different from XML Web Service)
```go
[OperationContract]
[WebGet(ResponseFormat = WebMessageFormat.Json, UriTemplate = "/method1/{Parameter1}/{Parameter2}/{Parameter3}")]
void method1(string Parameter1, string Parameter2, string Parameter3);
```

# Reference Link
* http://web.csulb.edu/~pnguyen/cecs475/pdf/cshtp5_30.pdf
