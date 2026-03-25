Lespion — Insider Threat OSINT Investigation

Analyst: Lakresha Stout
Platform: CyberDefenders
Category: Threat Intelligence / OSINT
Tools Used: GitHub, Instagram, Google Search, Google Image Search, CyberChef, ZXing QR Decoder


Scenario
A client reported a network compromise that resulted in systems being taken offline. Initial findings from incident responders indicated that the activity originated from a single internal user account, suggesting a potential insider threat. The objective of this investigation was to identify the insider, analyze their activity, and correlate available intelligence to build a profile of the attacker using open-source intelligence techniques and provided artifacts.


Investigation
The investigation began with analysis of the provided GitHub data associated with the suspected insider. During repository review, an exposed API key was discovered embedded directly within the source code. The key appeared to be hardcoded, which represents a significant security vulnerability. Hardcoded credentials allow unauthorized access to services and can be exploited by attackers if publicly accessible.

The exposed API key identified during repository analysis was:

aJFRaLHjMXvYZgLPwiJKroYLGRkNBW

Further review of the repository revealed a Base64 encoded string associated with authentication logic. The encoded value was extracted and decoded using CyberChef. The decoding process revealed a plaintext password, confirming that sensitive authentication data had been improperly stored within the repository.

The encoded string identified was:

UGljYXNzb0JhZ3VldHRloTk=

Decoding the value produced the plaintext password:

PicassoBaguette99

This confirmed that credentials were both exposed and weakly protected, increasing the likelihood of unauthorized access.
Repository analysis continued to identify suspicious tooling associated with the insider. While reviewing repository descriptions, references to cryptocurrency mining activity were observed, specifically mentioning CryptoNight. The repository README file was examined, which identified XMRig as the mining backend. XMRig is a cryptocurrency mining tool commonly associated with cryptojacking and unauthorized resource usage. The presence of this software suggested potential malicious intent and misuse of computing resources.

Following repository analysis, open-source intelligence techniques were used to correlate the insider’s online presence. The username associated with the GitHub account was used to pivot across online platforms. This research led to the discovery of a gaming profile associated with the insider on the Steam platform. The discovery of this account provided additional confirmation of the suspect’s online identity.

Further investigation involved performing a Google search using the insider’s username. This search led to the identification of an Instagram profile associated with the same user:

https://www.instagram.com/emarseille99/

The Instagram account provided additional intelligence, including travel photos and personal information that could be used to build a more complete profile of the suspect.

Images posted to the Instagram profile were analyzed using reverse image search techniques. One image indicated that the insider had recently traveled for a holiday. The image was uploaded to Google Image search, which identified the location as Singapore.

Additional images on the profile were reviewed for further intelligence. Another image referencing the insider’s family was analyzed using reverse image search. The results indicated that the family resided in Dubai. This provided another geographic connection related to the insider.

The investigation then shifted to the provided artifacts within the investigation package. The file labeled office.jpg contained an image of a building associated with the company. This image was uploaded to Google Image search to determine the location. The reverse image search matched the building to a location in Birmingham, identifying the city where the company office was located.
Finally, the provided surveillance image labeled Webcam.png was analyzed. The image appeared to show a landmark captured from a surveillance camera. The image was uploaded to Google Image search, and the location was identified. The landmark corresponded to a location in the state of Indiana, establishing the suspect’s final observed travel destination.


Findings
The investigation identified exposed credentials, suspicious cryptocurrency mining software, and multiple online accounts associated with the insider. OSINT techniques were used to correlate social media activity and analyze images to determine travel locations and geographic associations. The suspect was linked to travel in Singapore, family connections in Dubai, a company office located in Birmingham, and surveillance imagery placing them in Indiana. These findings collectively established a comprehensive profile of the insider and demonstrated how publicly available intelligence can be used to track malicious actors.
