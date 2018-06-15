## Adding Table for Queues
Let’s now add the table for queued jobs and failed job. For that, run the following Artisan commands:
```
php artisan queue:table
php artisan queue:failed-table
```
## Migrating the Tables
Now that I have all the required tables, let’s now migrate it using the following Artisan command:
```
php artisan migrate
```
## Updating the .env File
I will now update the .env file with the mail and the queue driver information. I will use Gmail to send verification emails. Following are the changes in the .env file:

```
QUEUE_DRIVER=database
MAIL_DRIVER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=adineshpithiya@gmail.com
MAIL_PASSWORD=
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=dineshpithiya@gmail.com
MAIL_FROM_NAME="dinesh pithiya"
```

## Creating Mail And the View For Email Verifications
Since I have setup SMTP to send emails, it’s time to create an email class which will return view and token to be sent with the email. Run the following Artisan command to create an email setup:

```
php artisan make:mail EmailVerification
```
Once the command finishes, a new folder with the name Mail along with the EmailVerification class file will be created inside the app folder. This is the file I need to call when I need to send an email.

## Updating the EmailVerification Class
This class comes with two methods. The first is the constructor() and the second is the build() which will do most of the work. It binds the view with the email. I will send a user token along with the view so that the user can be verified. For this, add a new protected variable in the class with the name 

```
php artisan make:mail OrderShipped
```
## add below script into your mail class
> \app\Mail\OrderShipped.php
```
public function build()
{
	return $this->from('dinesh@moontechnolabs.com')->view('welcome');
}
```

## add dependency into your controller
```
use Illuminate\Support\Facades\Mail;
use App\Mail\OrderShipped;
```

## Let's send mail
```
Mail::to('dinesh@gmail.com')->queue(new OrderShipped());
```

## verify your Quelist
```
php artisan queue:listen
```

More information you can follow
[Click here to more information link](https://hackernoon.com/how-to-use-queue-in-laravel-5-4-for-email-verification-3617527a7dbf)
