**Comment JSON Data Structure for Opensearch Overview**

The purpose of this record shown below is to have the vital information needed for the index within Opensearch that will deal with the comments and it's attachments. The docketID is there so that we will be able to associate the single or many comments to the docket that it is associated with. The commentID is there so that we can associate the comment to the attachments that are associated with it. The commentText will allow us to know if there are any attachments associated with the comment. This leads to the attachmentText which is the extracted text from the attachments associated with a comment.

```json
{
  "docketID": "DOS-2022-0004",
  "commentID": "DOS-2022-0004-0003",
  "commentText": "See attached file(s)",
  "attachmentsText": [
     {
       "Text": "Raytheon Technologies\n870 Winter St.\nWaltham, MA 02451-1449..."
     }
  ]
}
```

**Explanation of JSON Data Structure**

The JSON key-value pairs above are derived from the comment JSON file that is associated with a docket as shown in the screen shots below. 

![Screenshot 2025-01-30 at 11 46 14 AM 2](https://github.com/user-attachments/assets/eef19aa1-47a5-407f-ab9b-c4ec9c6bd41d)
![Screenshot 2025-01-30 at 11 46 14 AM](https://github.com/user-attachments/assets/ff4bc8a9-9f28-47ae-892d-334d10dffeb7)


On the other hand the "Text" key-value pair is derived from the extracted text from the attachments associated with the comment. This is how we can associate the attachments to the comments. 

*Portion of extracted text below:*

<img width="745" alt="Screenshot 2025-01-30 at 11 48 03 AM" src="https://github.com/user-attachments/assets/68a97f42-842e-4418-9fcb-4d5085258a8f" />


**Example DocketID:** DOS-2022-0004

<img width="596" alt="DOS-2022-0004_tree" src="https://github.com/user-attachments/assets/52342a58-3a4c-45f3-a202-d51d412212d0" />



In the docket above we can see all of the files associated with this specific docket. Under the "Comments" folder there are two comments associated with this docket and we know from the "comments_attachments" folder that there are two pdf attachments.

Looking closer at the attachments we can see that each attachment has a commentID associated with it. This is how we can associate the attachments to the comments. 

For example, the attachment "DOS-2022-0004-0003.pdf" has a commentID of "DOS-2022-0004-0003". This is how we can associate the attachments to the comments. 

The "comments_extracted_text" folder contains the extracted text from the attachments. This is what will be used to populate the "attachmentsText" field in the record above.

**Opensearch Data Structure**

When talking about Opensearch think of it as a database. The record above is how we would like the data to be structured in the database. This will allow us to associate the comments to the docket and the attachments to the comments. Our goal is to achieve an output that would be similarly represented in SQL as shown below.

```
Fake SQL Query:

SELECT docketID, count(commentText) AS termCount
FROM comments 
WHERE commentText LIKE ‘%’ || term  || ‘%’ 
GROUP BY docketID;
```

Essentially we are looking to create a search engine that when given a term will return the dockets that match the term and the number of times the term appears in the comments associated with the docket. Furthermore, we would like to see the attachments associated with the comments that contain the term as well. Thus, leading us to structure the JSON as shown above to create a single record that will be indexed in Opensearch.



