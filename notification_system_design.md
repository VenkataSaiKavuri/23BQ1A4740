Stage 1

Core Actions Supported

  1. Get notifications
  2. Mark notification as read
  3. Delete notification
  4. Receive real-time notifications

Get Notifications

Endpoint

  GET /evaluation-service/notifications
  
Headers

  Authorization: Bearer <token>
  Content-Type: application/json

Response

    
    {
      "notifications": [
        {
          "id": "550e8400-e29b-41d4-a716-446655440000",
          "type": "TASK_ASSIGNED",
          "message": "New task assigned",
          "timestamp": "2026-06-05T10:00:00"
        }
      ]
    }

Mark Notification As Read

Endpoint

  PATCH /evaluation-service/notifications/{id}/read

Response

    {
        "message": "Notification marked as read
    }

Delete Notification

Endpoint

  DELETE /evaluation-service/notifications/{id}

Response

    {
      "message": "Notification deleted successfully"
    }

Stage 2

Persistent Storage Choice

SQLite3 is used as the persistent storage database and Django ORM is used for database interactions.

Reason

  . Lightweight and easy to configure.
  . No separate database server required.
  . Suitable for development and assessment environments.
  . Fully supported by Django ORM.

Database Schema

Notification Model

    class Notification(models.Model):
        id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
        student_id = models.IntegerField()
        type = models.CharField(max_length=100)
        message = models.TextField()
        isRead = models.BooleanField(default=False)
        createdAt = models.DateTimeField(auto_now_add=True)

SQL Representation

    CREATE TABLE notifications (
        id UUID PRIMARY KEY,
        student_id INTEGER NOT NULL,
        type VARCHAR(100) NOT NULL,
        message TEXT NOT NULL,
        isRead BOOLEAN DEFAULT FALSE,
        createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );

Queries Used By APIs

Get Notifications

    SELECT * FROM notifications WHERE student_id = 1111 AND isRead = false ORDER BY createdAt DESC;

Mark Notification As Read

    UPDATE notifications SET isRead = true WHERE id = '<notification_id>';

Scaling Challenges

Problem 1

Large numbers of notifications can slow query performance.

Solution

Create indexes on:

    student_id
    isRead
    createdAt

Problem 2

Database size grows over time.

Solution

Archive old notifications.

Problem 3

SQLite supports limited concurrent writes.

Solution

Migrate to PostgreSQL for production-scale workloads.
