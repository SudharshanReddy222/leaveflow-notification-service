
# LeaveFlow Notification Service - Copilot Instructions

## Project Overview
This microservice consumes Kafka events and manages employee notifications.

## Technology Stack
- Java 21
- Spring Boot 3.5.x
- Spring for Apache Kafka
- Spring Data JPA
- PostgreSQL
- Maven
- Lombok
- JUnit 5
- Mockito

## Port
8084

## Database
Database name: leaveflow_notification

This service owns notification records.

## Package Structure
com.leaveflow.notification
controller
service
repository
entity
dto
event
response
exception
config
kafka

## Responsibilities
1. Consume leave-related Kafka events.
2. Process approval and rejection notifications.
3. Persist notifications in PostgreSQL.
4. Expose APIs to retrieve employee notifications.
5. Prevent duplicate event processing.

## Kafka Configuration

Topic:
leave-events

Consumer group:
notification-service-group

Supported event types:
- LEAVE_APPROVED
- LEAVE_REJECTED

Expected event structure:
- eventId
- eventType
- leaveId
- employeeId
- employeeEmail
- message
- eventTimestamp

Use JSON deserialization into a dedicated event DTO.

Do not use Java native serialization.
Do not trust arbitrary incoming event data without validation.

## Notification Entity
Fields:
- id (Long)
- eventId (String, unique)
- employeeId (Long)
- leaveId (Long)
- notificationType (String)
- message (String)
- read (Boolean)
- createdAt (LocalDateTime)

## Kafka Consumer Requirements
- Use @KafkaListener.
- Configure consumer group.
- Configure JSON deserialization.
- Handle malformed messages.
- Prevent duplicate processing using eventId.
- Handle database failures correctly.
- Configure retry and dead-letter handling where practical.
- Do not acknowledge messages before successful processing.
- Avoid losing messages during processing failures.

## API Endpoints
GET /api/notifications/{employeeId}

GET /api/notifications/{employeeId}/unread

PUT /api/notifications/{id}/read

## Coding Standards
- Use constructor injection.
- Follow layered architecture.
- Use DTOs.
- Use transactions where needed.
- Use custom exceptions.
- Implement global exception handling.
- Keep Kafka consumer logic separate from business logic.

## Security
- APIs must only be accessible through the API Gateway.
- Employees can view only their own notifications.
- Managers and admins can access only authorized notifications.
- Never trust employeeId from an unverified client request.

## Testing
Test:
- Successful Kafka event consumption.
- JSON deserialization.
- Duplicate event handling.
- Database persistence.
- Invalid event handling.
- Notification retrieval.
- Mark notification as read.
- Kafka retry and failure scenarios.

## Important
Inspect existing project files before implementation.
Do not hardcode Kafka or database credentials.
Run Maven tests after modifications.
