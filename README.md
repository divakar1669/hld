# High-Performance URL Shortening Service Design 🚀

![Alt Text](https://i.imgur.com/MknxWZQ.gif)

## Functional Requirements:
- Create a short URL for a given URL.
- Keep the generated URL as short as possible (approx. 7-16 characters).
- Ensure lifetime availability and instant redirection.

## Non-Functional Requirements:
- Ensure high availability and consistency.
- Achieve super-low latency for redirection.

## Design Estimates:
Let's scale this up:
- Serving 50 million users.
- Generating 2 million URLs daily.
- Handling 20 million daily reads.
- Read:Write ratio of 10:1.

**Storage:**
- Disk
  - Shortened URL: tinyurl.com/abcd123 (avg. 15B)
  - Original URL: https://www.google.com/search?q=who+will+be+the+next+PM+of+india (100B)
  - Unique ID: 20B
  - Total = 200B per URL
- Every Year: 146TB

**Cache:**
- 20% of URLs attract high traffic
- Cache Size: 30TB (approx.)

## APIs:
- POST: `api/create?url={long_url}` => Response: `short_url`
- GET: `{short_url}` => Redirect to `long_url`

## URL Generation Techniques:
- A. Use Unique ID Generated by DB
- B. UUID Generation
- C. MD5 Hash Technique
- D. Base 62 Key Generation

**A. Use Unique ID Generated by DB** - Utilizing a unique ID generated by the database is an option, but it typically results in a 32-bit integer, which may pose scalability challenges. This approach requires a centralized ticket generation system to maintain consistency.

**B. UUID Generation** - UUIDs are 128-bit IDs generated by the system. While they are highly unique, they can be relatively long to store and have potential limitations when dealing with duplicity in distributed systems.

**C. MD5 Hash Technique** - MD5 hashing generates a 128-bit integer, but the first 7 characters in the resulting hash are often the same. This characteristic does not align well with the requirement for providing short 7-character URLs.

**D. Base 62 Key Generation** - Base 62 key generation offers approximately 3,500 billion unique combinations, making it a highly accurate method. It is easily scalable, and there is no need to worry about the scaling process as ranges can be defined.

**Base 62 Key Generation** is the winner for accuracy, scalability, and avoiding duplicates.

## High-Level Design Overview:

### API Endpoints:
- POST `api/create`: Create a short URL from a long one.
- GET `{short_url}`: Redirect to the original long URL.

### URL Shortening Technique:
- Base 62 Encoding: Combining 62 characters (A-Z, a-z, 0-9) for unique identifiers.
# URL Shortening Service README

This project is a URL shortening service that uses Base 62 encoding to create unique identifiers for long URLs. It provides API endpoints for shortening long URLs and redirecting to the original ones. Here's a brief overview of the project's key components and design considerations:

## API Endpoints

### POST api/create
- Receives a long URL and returns a shortened URL.

### GET {short_url}
- Redirects to the original long URL.

## URL Shortening Technique

We use Base 62 encoding, utilizing a combination of 62 characters (A-Z, a-z, 0-9) to create a unique identifier for each URL.

## Scalability and High Availability

To ensure scalability and high availability, we implement the following strategies:
- Load Balancers: We use load balancers to distribute traffic evenly across multiple servers.
- Caching: Implement caching mechanisms for frequently accessed URLs to reduce latency and improve performance.
- Distributed Database: We plan to use a distributed database system to handle large-scale data and ensure availability.

## Redundancy and Backup

For data redundancy and backup, we follow these practices:
- Regular Backups: We perform regular backups of our data.
- Data Replication: Data is replicated across multiple data centers for disaster recovery.

## Apache ZooKeeper

We utilize Apache ZooKeeper for maintaining microservices and managing partition keys across geographic locations. This ensures coordination and consistency in a distributed environment.

## URL Archiving

To manage old or stale URLs, we implement a process for archiving. We define criteria for determining when a URL becomes stale and establish an archiving mechanism to free up storage resources while preserving access to historical URLs.

## Wrap Up:
In summary, the designed URL shortening service offers robust API endpoints for URL shortening and redirection. It utilizes Base 62 encoding for compact unique identifiers and incorporates scalability measures, redundancy, and microservices coordination with Apache ZooKeeper.

👉 **Try Building Your Own! Check out Catalyst's hands-on tutorial:** [Tiny URL Generation App Tutorial](https://docs.catalyst.zoho.com/en/tutorials/catly/nodejs/introduction/)


🔗 **Links to Popular URL Shorteners:**
- [Bitly](https://bitly.com/)
- [Bl.ink](https://www.bl.ink/)
- [TinyURL](https://tinyurl.com)

Get ready to create your URL shortening service! 🚀 #TechDesign #URLShortener #HighPerformance



