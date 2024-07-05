# PointingPoker
Create a pointing poker application. https://en.wikipedia.org/wiki/Planning_poker

Business Requirements:
	• Users should be able to create and start a pointing poker session
		○ Require point values and name
		○ Require pointing poker session name
		○ Add an option for general popular templates (EXTRA)
	• Using a given link or code number, users should be able to join a pointing poker session
		○ User should be required to enter a name
	• Users should be able to select a point value and will receive confirmation when other individuals select but not their actual value.
	• When any user clicks 'reveal', it should show all point values from each user and the average.
	• Clicking 'reset' will reset everybody's pointing poker status to the default. 
	• When all users have left the pointing poker session, the session will automatically close

Backend specifications (REST)
	• /api/create (POST)
		○ Creates a new pointing poker session
		○ Request body
		{
			sessionName: string,
			pointType: {
				'Three': integer,
				'Four': integer
			},
			userName: string
		}
		○ Response body
			§ 200 (SUCCESS)
		{
			sessionNumber: integer;
		}
	• /api/{sessionNumber}/join (POST)
		○ Allow user to join existing pointing poker session
		○ Request body
		{
			name: string
		}
		○ Response body
			§ 200 (SUCCESS)
		{
			userName: string
		}
			§ 500 (INTERNAL SERVER ERROR)
		{
			errorMessage: user name taken
		}
	• Pub/Sub
		○ To ensure all clients actively retrieve the latest data without having to constantly call 'GET' calls, we'll try to use a technology called 'Pub/Sub'
			§ We'll be using google's pub/sub service (They have a free tier)
			§ https://cloud.google.com/pubsub/docs/publish-receive-messages-console
	• /api/{sessionNumber}/point (POST)
		○ Allow user to submit pointing estimate
		○ Request body
		{
			userName: string,
			pointValue:  integer
		}
		○ 204 (RESOURCE CREATED)

Database design
	• Session table
		○ Session number (PRIMARY KEY)
		○ Session name
	• User table
		○ User name (PRIMARY KEY) <- stored in cookies
		○ Session number (PRIMARY KEY, FOREIGN KEY)
		○ Pointing value
