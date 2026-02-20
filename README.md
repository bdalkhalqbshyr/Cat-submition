# Cat-submition
Technical task
#include <iostream>
#include <string>
using namespace std;

/* =========================
   1) Interface
========================= */
class INotificationChannel {
public:
    virtual void send(string message) = 0;
    virtual ~INotificationChannel() {}
};

/* =========================
   2) Concrete Classes
========================= */

class EmailNotification : public INotificationChannel {
public:
    void send(string message) override {
        cout << "[Email] Sending: " << message << endl;
    }
};

class SMSNotification : public INotificationChannel {
public:
    void send(string message) override {
        cout << "[SMS] Sending: " << message << endl;
    }
};

class PushNotification : public INotificationChannel {
public:
    void send(string message) override {
        cout << "[Push] Sending: " << message << endl;
    }
};

/* =========================
   3) Factory Pattern
========================= */

class NotificationFactory {
public:
    static INotificationChannel* createChannel(string type) {

        if (type == "email")
            return new EmailNotification();

        else if (type == "sms")
            return new SMSNotification();

        else if (type == "push")
            return new PushNotification();

        else
            return nullptr;
    }
};

/* =========================
   4) User Preferences
========================= */

class UserPreferences {
public:
    bool emailEnabled;
    bool smsEnabled;
    bool pushEnabled;

    UserPreferences(bool e, bool s, bool p) {
        emailEnabled = e;
        smsEnabled = s;
        pushEnabled = p;
    }
};

/* =========================
   5) Notification Manager
========================= */

class NotificationManager {
public:
    void sendNotification(string message, UserPreferences pref) {

        if (pref.emailEnabled) {
            INotificationChannel* email =
                NotificationFactory::createChannel("email");

            if (email) {
                email->send(message);
                delete email;
            }
        }

        if (pref.smsEnabled) {
            INotificationChannel* sms =
                NotificationFactory::createChannel("sms");

            if (sms) {
                sms->send(message);
                delete sms;
            }
        }

        if (pref.pushEnabled) {
            INotificationChannel* push =
                NotificationFactory::createChannel("push");

            if (push) {
                push->send(message);
                delete push;
            }
        }
    }
};

/* =========================
   Main
========================= */

int main() {

    string message;
    cout << "Enter notification message: ";
    getline(cin, message);

    // مثال: المستخدم عايز Email و Push بس
    UserPreferences user(true, false, true);

    NotificationManager manager;
    manager.sendNotification(message, user);

    return 0;
}
