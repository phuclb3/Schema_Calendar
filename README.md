{
  "openapi": "3.1.0",
  "info": {
    "title": "Calendar Management API",
    "version": "1.0.0",
    "description": "This API allows creating new events or querying calendar data based on user input. It supports relative date expressions (e.g., 'hôm nay', 'mai', 'mốt', 'kia') and defaults time to 00:00 or 23:59 if not specified."
  },
  "servers": [
    {
      "url": "https://hook.eu2.make.com",
      "description": "Base webhook domain"
    }
  ],
  "paths": {
    "/Link webhook1": { 
      "get": {
        "summary": "Create a new calendar event",
        "operationId": "createCalendarEvent",
        "parameters": [
          {
            "name": "date",
            "in": "query",
            "required": true,
            "description": "The date of the event in DD/MM/YYYY format or a relative expression (e.g., 'hôm nay', 'mai', 'mốt', 'kia'). The system will parse it into an actual date.",
            "schema": {
              "type": "string",
              "example": "06/02/2025"
            }
          },
          {
            "name": "time",
            "in": "query",
            "required": false,
            "description": "The time of the event in HH:MM format. If not provided, the event is considered all-day (00:00 - 23:59).",
            "schema": {
              "type": "string",
              "example": "11:30"
            }
          },
          {
            "name": "datetime",
            "in": "query",
            "required": true,
            "description": "The ISO 8601 formatted datetime of the event (e.g., '2025-02-06T11:30:00+07:00'). If `time` is not provided, this field can represent the start of the day (00:00) or an all-day block. The backend may override or adjust this value based on the date/time parsing logic (including relative dates).",
            "schema": {
              "type": "string",
              "example": "2025-02-06T11:30:00+07:00"
            }
          },
          {
            "name": "content",
            "in": "query",
            "required": true,
            "description": "The details or description of the event.",
            "schema": {
              "type": "string",
              "example": "Họp với đối tác"
            }
          },
          {
            "name": "location",
            "in": "query",
            "required": true,
            "description": "The location where the event will take place.",
            "schema": {
              "type": "string",
              "example": "tại Quán cafe The Fox"
            }
          },
          {
            "name": "notification",
            "in": "query",
            "required": true,
            "description": "When app send the notification for event",
            "schema": {
              "type": "number",
              "example": "15"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "The event was successfully created.",
            "content": {
              "application/json": {
                "schema": {
                  "type": "object",
                  "properties": {
                    "status": {
                      "type": "string",
                      "example": "success"
                    },
                    "event_id": {
                      "type": "string",
                      "example": "evt_12345"
                    }
                  }
                }
              }
            }
          },
          "400": {
            "description": "Invalid input parameters."
          }
        }
      }
    },
    "/Link webhook 2": { 
      "get": {
        "summary": "Query calendar events based on specific parameters",
        "operationId": "queryCalendarEvents",
        "parameters": [
          {
            "name": "datestart",
            "in": "query",
            "required": false,
            "description": "Tách dữ liệu thành 0h00 (hoặc giờ cụ thể nếu có) của ngày tìm kiếm theo chuẩn ISO 8601",
            "schema": {
              "type": "string",
              "example": "2025-02-07T00:00:00+07:00"
            }
          },
          {
            "name": "dateend",
            "in": "query",
            "required": false,
            "description": "Tách dữ liệu thành 23h59 (hoặc giờ cụ thể nếu có) của ngày tìm kiếm theo chuẩn ISO 8601",
            "schema": {
              "type": "string",
              "example": "2025-02-07T23:59:00+07:00"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Successfully retrieved calendar events matching the query.",
            "content": {
              "application/json": {
                "schema": {
                  "type": "object",
                  "properties": {
                    "events": {
                      "type": "array",
                      "items": {
                        "type": "object",
                        "properties": {
                          "date": {
                            "type": "string",
                            "description": "The date in DD/MM/YYYY format.",
                            "example": "07/02/2025"
                          },
                          "time": {
                            "type": "string",
                            "description": "The time in HH:MM format and plus 07:00 .",
                            "example": "10:00+07:00"
                          },
                          "datetime": {
                            "type": "string",
                            "description": "The ISO 8601 datetime.",
                            "example": "2025-02-07T10:00:00+07:00"
                          },
                          "title": {
                            "type": "string",
                            "description": "The title or main content of the event.",
                            "example": "Họp với khách hàng"
                          },
                          "location": {
                            "type": "string",
                            "description": "The location of the event.",
                            "example": "Phòng họp B"
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          },
          "400": {
            "description": "Invalid input. Unable to process the query."
          }
        }
      }
    }
  }
}
