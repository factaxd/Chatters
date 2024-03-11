# Chatters
This Android application is a social media app that allows users to chat with each other and create user profiles. The app is developed using Firebase Authentication and Firestore.


Login and Registration: Users login or register via their phone numbers.
OTP Verification: OTP (One-Time Password) is sent using Firebase Authentication for phone number verification.
User Profile Creation: After logging in, users can create profile information such as username and profile picture.
Friend Search: Users can search for other users and send friend requests.
Chat: Users can initiate a chat with other users and share messages. Chats are stored in Firestore.
Notifications: Users can see messages or friend requests from other users via notifications.

The app offers a user-friendly interface and provides an interactive social media experience based on Firebase's cloud-based services. Additionally, it keeps users up-to-date through notifications and enables real-time chatting.

Keywords: Firebase, Android Programming, OTP Verification, User Profile



Login and Registration:
Firebase Authentication is a robust identity verification service used for login and registration processes. The relevant codes of the project demonstrate how Firebase Authentication is integrated, and how phone number verification and login procedures are performed.



1.2. Phone Number Verification Process:

countryCodePicker.registerCarrierNumberEditText(phoneInput);
In this line, the countryCodePicker object is associated with the EditText component named phoneInput. This association is used to facilitate country code selection and phone number validation.

sendOtpBtn.setOnClickListener((v)->{...});
In this line, a click event is assigned to a button named "sendOtpBtn". When the click event occurs: If the entered phone number is invalid, an error message is displayed. If the phone number is valid, a transition to the "LoginOtpActivity" activity is made, and the phone number information is passed to this activity.




1.3. Setting Username:

void setUsername(): This function is used to set the user's username. The function includes the following steps: The username is retrieved from the EditText component named usernameInput. The validity of the entered username is checked; if it is empty or less than 3 characters, an error message is displayed, and the function is terminated. A progress indicator is made visible with the call to setInProgress(true). If a userModel has been previously created, the username is set on the userModel; otherwise, a new UserModel instance is created. The user information is saved to the Firestore database with the call FirebaseUtil.currentUserDetails().set(userModel). Upon completion of the asynchronous operation, if successful, the progress indicator is hidden with the call to setInProgress(false), and an intent is started to redirect the user to MainActivity.

void getUsername(): This function is used to retrieve the current username of the user. The function includes the following steps: A progress indicator is made visible with the call to setInProgress(true). The current user information is retrieved from Firestore and converted to a UserModel instance named userModel. If userModel exists, the username is assigned to the EditText component usernameInput. The progress indicator is hidden with the call to setInProgress(false).




2.1. Chat:

csuper.onCreate(savedInstanceState); and setContentView(R.layout.activity_chat);
These lines initiate the creation process of the activity by calling the superclass's onCreate method and determine the interface of the activity by loading the layout named "activity_chat.xml".

otherUser = AndroidUtil.getUserModelFromIntent(getIntent());
In this line, the information of the other user (otherUser) is obtained using the getUserModelFromIntent function from the AndroidUtil class. The user model is extracted from the information coming from the intent.

chatroomId = FirebaseUtil.getChatroomId(FirebaseUtil.currentUserId(),otherUser.getUserId());
In this line, the chatroom ID (chatroomId) is created. The getChatroomId function in the FirebaseUtil class generates a unique chatroom ID using the IDs of the current user and the other user.

messageInput, sendMessageBtn, backBtn, otherUsername, and recyclerView are defined. These components represent different elements on the interface of the activity.

otherUsername.setText(otherUser.getUsername());
In this line, the name of the other user (otherUser.getUsername()) is assigned to the otherUsername TextView component.

sendMessageBtn.setOnClickListener((v -> {...}));
In this line, a click event is assigned to the "Send Message" button. When the click event occurs, the entered message is retrieved, and the sendMessageToUser function is called to send the message.

getOrCreateChatroomModel();
In this line, the getOrCreateChatroomModel function is called to retrieve or create the model of the current chatroom.

setupChatRecyclerView();
In this line, the setupChatRecyclerView function is called to configure the RecyclerView component for displaying chat messages.





2.2. Sending a Message to the User

Updating Chatroom Information:
The information within the chatroomModel is updated. LastMessageTimestamp is set with an updated timestamp. LastMessageSenderId is set with the ID of the user sending the message. LastMessage is set with the sent message. These details are updated by writing to the relevant chatroom reference (chatroomId) in the Firebase database.

Adding Message to Firebase Database:
A ChatMessageModel containing the information of the sent message is created. This model is added to the chat messages collection reference in the Firebase database (FirebaseUtil.getChatroomMessageReference(chatroomId)). The addition process is listened to with addOnCompleteListener, and if successfully added, the messageInput field where the user entered the message is cleared (messageInput.setText("")) and sendNotification function is called to send a notification.

Retrieving Chatroom Model from Firebase Database:
Access to a specific chatroom reference is made using FirebaseUtil.getChatroomReference(chatroomId). A Firebase query is executed using the get method to retrieve the data at this reference. A listener is added with addOnCompleteListener when the query is completed.

If the Query is Successful:
If the query is successful, task.isSuccessful() is checked. The data retrieved from the query result is converted to a ChatroomModel class using task.getResult().toObject(ChatroomModel.class). If there is a chatroom model (chatroomModel), it is directly assigned to the chatroomModel variable.

Creating Chatroom Model if it Doesn't Exist:
If the chatroom model (chatroomModel) is null, it means that a chat is being initiated for the first time. A new ChatroomModel is created, and necessary information (chatroom ID, participants, start timestamp, last message) is assigned to this model. This created model is written back to the same chatroom reference in the Firebase database. This function retrieves or creates the current model of a specific chatroom and updates this model.





3.1. User Search

Creating Firebase Firestore Query:
A reference to the Firestore collection containing all users is obtained using FirebaseUtil.allUserCollectionReference(). Using the whereGreaterThanOrEqualTo and whereLessThanOrEqualTo methods, user names are filtered based on a specific search term. This condition means that the username should be greater than or equal to and less than or equal to the search term. The \uf8ff character represents the highest character in the Unicode character set, including all characters after a specific character string.

Creating FirestoreRecyclerOptions:
A FirestoreRecyclerOptions object is created with FirestoreRecyclerOptions.Builder. This object determines how the results obtained from the Firestore query will be connected to the RecyclerView. It is configured to process these results using the UserModel class.

Creating an Instance of SearchUserRecyclerAdapter:
An instance of the SearchUserRecyclerAdapter class is created. This adapter takes the FirestoreRecyclerOptions object and the application's context as parameters.

Setting Up RecyclerView:
A layout manager is assigned to the RecyclerView using recyclerView.setLayoutManager to use a vertical linear layout in this case. An adapter is set to the RecyclerView using recyclerView.setAdapter. This adapter is configured to display user data obtained from the Firestore query. The adapter.startListening method starts listening to Firestore data and ensures the RecyclerView is updated.





3.2. Updating Profile

Username Validation:
The newUsername variable contains the new username entered by the user. It is checked if the entered username is empty or shorter than three characters. If these conditions are not met, an error message is set with usernameInput.setError, and the function is terminated.

Updating Username:
The currentUserModel.setUsername(newUsername) updates the username in the user's data model.

Setting Progress Status:
setInProgress(true) initiates a progress status display during the operation. When the loading process is completed, the updateToFirestore() function is called within addOnCompleteListener.

Updating Firestore:
The updateToFirestore() function saves the user's updated information in the Firestore database. setInProgress(false) closes the progress status display.





4. Notification Handling

The FCMNotificationService class represents a service class that listens for notifications coming through the Firebase Cloud Messaging (FCM) service. FCM is a cloud-based notification framework that enables communication between servers and devices. This service runs in the background of the application and captures notifications coming from FCM servers. The main tasks of this class are:

Handling Notifications: By overriding the onMessageReceived method, it processes notifications coming from FCM servers. This method is used to receive notifications even when the application is running in the background or closed.

Handling Notification Clicks: By overriding the onNewToken method, it updates a device-specific FCM token. This token helps identify the device and correctly route notifications.
