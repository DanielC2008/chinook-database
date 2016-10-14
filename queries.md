
Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.

SELECT Customer.FirstName || " " || Customer.LastName AS "FullName", CustomerId, Country From Customer
WHERE Country != 'USA'

Provide a query only showing the Customers from Brazil.

SELECT * From Customer
WHERE Country = 'Brazil'

Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.

SELECT Customer.FirstName || ' ' || Customer.LastName AS 'FullName', InvoiceId, InvoiceDate, BillingCountry  FROM Invoice
JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
WHERE Customer.Country = 'Brazil'

Provide a query showing only the Employees who are Sales Agents.

SELECT * FROM Employee
WHERE Title = 'Sales Support Agent'

Provide a query showing a unique list of billing countries from the Invoice table.

SELECT BillingCountry FROM Invoice
GROUP BY BillingCountry

Provide a query showing the invoices of customers who are from Brazil.

SELECT * FROM Invoice
JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
WHERE Customer.Country = 'Brazil'

Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.

SELECT Invoice.*, Employee.Title FROM Invoice
JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
JOIN Employee ON Customer.SupportRepId = Employee.EmployeeId
WHERE Employee.Title = 'Sales Support Agent'

Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.

SELECT Invoice.Total AS 'Invoice Total', Customer.FirstName || ' ' || Customer.LastName AS 'Customer Name', Customer.Country, Employee.FirstName || ' ' || Employee.LastName AS 'Employee Name' FROM Invoice
JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
JOIN Employee ON Customer.SupportRepId = Employee.EmployeeId

How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?

SELECT * FROM Invoice
WHERE InvoiceDate LIKE '2009%' OR InvoiceDate LIKE '2011%'

SELECT SUM(Total) AS 'Total 2009' FROM Invoice
WHERE InvoiceDate LIKE '2009%'

SELECT SUM(Total) AS 'Total 2011' FROM Invoice
WHERE InvoiceDate LIKE '2011%'


Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.

SELECT COUNT(InvoiceId)
FROM InvoiceLine
WHERE InvoiceId = 37

Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY

SELECT Invoice.InvoiceId, COUNT(InvoiceLine.InvoiceLineId) AS "Line Items"
FROM InvoiceLine
JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
GROUP BY Invoice.InvoiceId

Provide a query that includes the track name with each invoice line item.

SELECT Track.Name, InvoiceLineId FROM InvoiceLine
JOIN Track ON Track.TrackId = InvoiceLine.Trackid

Provide a query that includes the purchased track name AND artist name with each invoice line item.

SELECT Artist.Name, Track.Name, InvoiceLineId FROM InvoiceLine
JOIN Track ON Track.TrackId = InvoiceLine.Trackid
JOIN Album ON Album.AlbumId = Track.AlbumId
JOIN Artist ON Artist.ArtistId = Album.ArtistId

Provide a query that shows the # of invoices per country. HINT: GROUP BY

SELECT BillingCountry, Count(BillingCountry) AS '# of Invoices' FROM Invoice
GROUP BY BillingCountry

Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resultant table.

SELECT Playlist.Name, Count(Track.TrackId) AS 'Total Tracks' FROM Track
JOIN PlaylistTrack ON Track.TrackId = PlaylistTrack.TrackId
JOIN Playlist ON PlaylistTrack.PlaylistId= Playlist.PlaylistId
GROUP BY Playlist.Name

Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.

SELECT Track.Name, MediaType.Name, Album.Title, Genre.Name
FROM Track
JOIN MediaType ON Track.MediaTypeId = MediaType.MediaTypeId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Genre ON Track.GenreId = Genre.GenreId

Provide a query that shows all Invoices but includes the # of invoice line items.

SELECT Invoice.*, Count(InvoiceLine.InvoiceLineId) AS '# of Line Items'
FROM Invoice
JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
GROUP BY Invoice.InvoiceId

Provide a query that shows total sales made by each sales agent.

SELECT Employee.FirstName || " " || Employee.LastName AS "Name", Title, Count(Invoice.Total) AS "Total Sales"
From Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
WHERE Title = "Sales Support Agent"
GROUP BY "Name"


Which sales agent made the most in sales in 2009?

SELECT Employee.FirstName || " " || Employee.LastName AS "Name", Employee.Title, Count(Invoice.Total) AS "Total Sales"
From Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
WHERE Employee.Title = "Sales Support Agent" AND Invoice.InvoiceDate LIKE "2009%"
GROUP BY "Name"
ORDER BY "Total Sales" DESC
LIMIT 1

Which sales agent made the most in sales in 2010?

SELECT Employee.FirstName || " " || Employee.LastName AS "Name", Employee.Title, Count(Invoice.Total) AS "Total Sales"
From Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
WHERE Employee.Title = "Sales Support Agent" AND Invoice.InvoiceDate LIKE "2010%"
GROUP BY "Name"
ORDER BY "Total Sales" DESC
LIMIT 1

Which sales agent made the most in sales over all?

SELECT Employee.FirstName || " " || Employee.LastName AS "Name", Title, Count(Invoice.Total) AS "Total Sales"
From Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
WHERE Title = "Sales Support Agent"
GROUP BY "Name"
ORDER BY "Total Sales" DESC
LIMIT 1

Provide a query that shows the # of customers assigned to each sales agent.

SELECT Employee.FirstName || " " || Employee.LastName AS "Name", Count(CustomerId) AS "Total Customers"
FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
GROUP BY "Name"

Provide a query that shows the total sales per country. Which country's customers spent the most?

SELECT BillingCountry, COUNT(Total) AS "Total Sales"
FROM Invoice
GROUP BY BillingCountry

SELECT BillingCountry, COUNT(Total) AS "Total Sales"
FROM Invoice
GROUP BY BillingCountry
ORDER BY "Total Sales" DESC
LIMIT 1

Provide a query that shows the most purchased track of 2013.

SELECT Track.Name, SUM(InvoiceLine.TrackId) AS "Most Sold Track"
FROM Track
JOIN InvoiceLine ON InvoiceLine.TrackId = Track.TrackId
JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
WHERE Invoice.InvoiceDate LIKE "2013%"
GROUP BY Track.Name
ORDER BY "Most Sold Track" DESC
LIMIT 1

Provide a query that shows the top 5 most purchased tracks over all.

SELECT Track.Name, SUM(InvoiceLine.TrackId) AS "Most Sold Track"
FROM Track
JOIN InvoiceLine ON InvoiceLine.TrackId = Track.TrackId
JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
GROUP BY Track.Name
ORDER BY "Most Sold Track" DESC
LIMIT 5


Provide a query that shows the top 3 best selling artists.

Provide a query that shows the most purchased Media Type.
