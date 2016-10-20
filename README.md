# load-index-full-text-eads-into-Primo
Loading/Indexing Fulltext of EADs into Primo

Title: Loading/Indexing Fulltext of EADs into Primo

Created by:

Ryan Edwards
Digital Access and Systems Librarian
Getty Research Institute
Los Angeles, CA
reedwards@getty.edu

Date: October 20, 2016

Problem: We at the Getty Research Institute have over 400 published Encoded Archival Description (EAD) finding aid records, which we maintain in our open-source ArchivesSpace system and present to our library patrons via our home-grown static Collection Inventories and Finding Aids pages on our website: http://archives2.getty.edu:8082/xtf/search?browse-creator=first;sort=creator Some of the EADs are quite lengthy and we wanted to be able to index all of this rich contextual information directly from within our own Primo-local instance: http://primo.getty.edu/primo_library/libweb/action/search.do?vid=GRI   

Solution: We learned from Laura Morse, Director, Library Systems at Harvard University that Harvard found a way to index the fulltext of their finding aids in Primo, while deduping the EAD records with their Aleph records.  We recreated the dedupe normalization rules in our Primo Back Office (see xml file attachment) to dedupe on the Alma MMSID manually injected into the identifier attribute, which we manually added to our ead header ead id elements.  Working with my former supervisor, Joseph Shubitowski, initially, I set up our complex xml file splitter and file splitter parameters.  Then, I proceeded to set up our data source, figure out the rest of our complex xml display, search, facet, link, and miscellaneous back-end Primo normalization rules, set up our load and delete pipes, and scope value that we added to our default search scope and view.

Presentation: Here is a link to the streaming video of our WebEx session that we had on Monday, October 18, 2016: https://getty.webex.com/getty/ldr.php?RCID=8c9e8d67811b5db6d57a7002b9960f69 

Our Process: We are copying our EADs manually to Primo and running a copy file load pipe, which runs to the DEDUP stage.
 
Currently, I follow this process for loading our EADs into Primo:
 
1. Receive notification from our Special Collections Cataloging Manager or our Institutional Archives Cataloger that new EADs have been added or updated. (We have a shared spreadsheet that they update via our Enterprise Box.com account, which lists all our accession numbers for our published finding aids and the corresponding Alma MMSID for the MARC21 descriptive records that we have in Alma.  We set this up this way, so we can keep track of new and updated EADs and also for future automated purposes, should we get our python script working to automatically inject our EAD IDs with identifier attributes, which contain the EADs corresponding Alma MMSIDs (all the EADs have an “identifier” attribute in the <eadid> element, which contains the MMSID from Alma for the corresponding MARC21 record; the identifier attribute is used by Primo to dedup the incoming EAD in the pipe with the Alma MARC21 record already published in Primo.

2. Manually transfer the processed EADs on a separate server to my machine via FileZilla (On my machine, I have all of our approximately 400 plus EADs archived and they all have identifier an “identifier” attribute in the <eadid> element, which contains the MMSID from Alma for the corresponding MARC21 record; for the initial load, I had to insert the “identifier” attribute in the <eadid> element manually for all the 400 plus EADs)

3. For any new EADs, open each EAD up in Notepad++, compare it with the old version of the EAD that has the Alma MMS ID in the <identifier> tag in the EAD header on my pc.  Then, copy the Alma MMS ID in the <identifier> tag in the EAD header to the new version, save it, and replace the old file with the new one.

4. Use a SSH client to connect to our Primo database server, cd to our directory where the EADs are kept, and delete all the EADs by issuing the command: “rm *“ in the directory.

5. Log in to the Primo Back Office and run the delete pipe, which wipes out the EADs from Primo.

6. In the Primo Back Office, execute the Indexing_and_Hotswapping process, which deletes all the EADs from the Primo index and refreshes the Primo index in about 20-30 minutes.

7. Via the SSH client, transfer all the EADs from my Desktop to EAD directory and compress them by issuing the command: “gzip *.xml”

8. In the Primo Back Office, find the EAD load pipe, back date it 3 years to say March 1, 2013, save it and run it.

9. In the Primo Back Office, execute the Indexing_and_Hotswapping process, which loads all the EADs into the Primo index and refreshes the Primo index in about 20-30 minutes.


Special thanks go to my supervisor, Joe for helping me get this to work as well as several of my colleagues for their support:

Joseph Shubitowski, Former Head, Library Information Systems
Teresa Soleau, Head of Library Systems & Digital Collection Management
Ruth Cuadra, Business Applications Administrator
Joshua Gomez, Software Engineer Senior
Lawrence Olliffe, Software Engineer
