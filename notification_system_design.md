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

