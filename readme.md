<h2 align="center">
  King of Bots - Real-time Snake Battle Arena <br/>
</h2>

<p align="center">
  <img src="https://img.shields.io/badge/Vue.js-3.2.13-green?logo=vuedotjs" alt="Vue.js Version">
  <img src="https://img.shields.io/badge/Spring%20Boot-2.7.1-brightgreen?logo=springboot" alt="Spring Boot">
  <img src="https://img.shields.io/badge/Java-8+-orange?logo=java" alt="Java">
  <img src="https://img.shields.io/badge/MySQL-8.0-blue?logo=mysql" alt="MySQL">
  <img src="https://img.shields.io/badge/WebSocket-Real--time-red?logo=socketdotio" alt="WebSocket">
  <img src="https://img.shields.io/badge/Bootstrap-5.1.3-purple?logo=bootstrap" alt="Bootstrap">
  <img src="https://img.shields.io/badge/JWT-Authentication-yellow?logo=jsonwebtokens" alt="JWT">
</p>

<p align="center">
  A competitive multiplayer snake battle game with AI bot support, featuring real-time battles and microservices architecture.
</p>

## Overview

King of Bots is a competitive multiplayer game where players control snakes in a battle arena. Players can either play manually or create AI bots to compete automatically. The game features real-time battles, player rankings, match history, and a comprehensive bot management system.

## Project Structure

```
King-of-Bots/
├── web/                    # Frontend Vue.js application
│   ├── src/
│   │   ├── components/     # Reusable Vue components
│   │   ├── views/         # Page-level components
│   │   ├── store/         # Vuex state management
│   │   ├── router/        # Vue Router configuration
│   │   └── assets/        # Static assets and game scripts
│   └── package.json       # Frontend dependencies
├── backendcloud/
│   ├── backend/           # Main Spring Boot service (Port 3000)
│   │   ├── src/main/java/com/kob/backend/
│   │   │   ├── controller/    # REST API endpoints
│   │   │   ├── service/       # Business logic
│   │   │   ├── consumer/      # WebSocket handlers
│   │   │   ├── pojo/          # Data models
│   │   │   ├── mapper/        # Database mappers
│   │   │   └── config/        # Configuration classes
│   │   └── pom.xml           # Backend dependencies
│   ├── matchingsystem/    # Matchmaking microservice (Port 3001)
│   │   └── src/main/java/com/kob/matchingsystem/
│   └── botrunningsystem/  # Bot execution microservice (Port 3002)
│       └── src/main/java/com/kob/botrunningsystem/
├── acapp/                 # Alternative frontend (legacy)
└── README.md
```

## Features

### Core Gameplay
- **Real-time Snake Battles**: Two players compete simultaneously in a grid-based arena
- **Dynamic Map Generation**: Randomly generated symmetric maps with obstacles
- **Multiple Control Modes**: Manual control or AI bot automation
- **Live Game Spectating**: Real-time game state updates via WebSocket

### User Management
- **Secure Authentication**: JWT-based authentication system
- **User Registration & Login**: Complete account management
- **Player Ratings**: ELO-style rating system for competitive play
- **Profile Management**: User avatars and statistics

### Bot System
- **Custom AI Bots**: Create and manage custom AI bots with Java code
- **Bot Execution Engine**: Secure sandboxed bot execution environment
- **Code Editor**: Built-in code editor with syntax highlighting
- **Bot Testing**: Test bots against different opponents

### Game Features
- **Matchmaking System**: Automatic player matching based on skill ratings
- **Game Records**: Complete match history with replay functionality
- **Leaderboard**: Global player rankings
- **Match Analysis**: Detailed game statistics and move history

## Technology Stack

### Frontend
| Tech   | Description                     |
|--------------|---------------------------------|
| Vue 3        | Progressive JavaScript framework |
| Vue Router   | Client-side routing             |
| Vuex         | State management                |
| Bootstrap 5  | Responsive UI framework         |
| Ace Editor   | Code editing component          |
| WebSocket    | Real-time communication         |

### Backend Services
| Tech     | Description                           |
|----------------|---------------------------------------|
| Spring Boot    | Main application framework            |
| Spring Security| Authentication and authorization      |
| MyBatis Plus   | Database ORM                          |
| JWT            | Stateless authentication tokens       |
| WebSocket      | Real-time game communication          |
| MySQL          | Primary database                      |

### Microservices
| Service            | Description                      |
|--------------------|----------------------------------|
| Main Backend       | Core game logic and APIs         |
| Matching System    | Player matchmaking service       |
| Bot Running System | AI bot execution engine          |


## Architecture

The project follows a microservices architecture with clear separation of concerns:

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Main Backend   │    │  Matching       │
│   (Vue.js)      │◄──►│   (Spring Boot)  │◄──►│  System         │
│   Port: 8080    │    │   Port: 3000     │    │   Port: 3001    │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │  Bot Running    │
                       │  System         │
                       │   Port: 3002    │
                       └─────────────────┘
```

### Service Details
- **Frontend (Port 8080)**: Vue.js SPA serving the user interface
- **Main Backend (Port 3000)**: Core game logic, user management, and WebSocket server
- **Matching System (Port 3001)**: Player matchmaking and queue management
- **Bot Running System (Port 3002)**: AI bot code execution and sandboxing
- **Database (Port 3306)**: MySQL database for persistent storage

### Database Schema

#### Users Table
| Field    | Type         | Description                    |
|----------|--------------|--------------------------------|
| id       | INT          | Primary key, auto increment    |
| username | VARCHAR(100) | Unique username                |
| password | VARCHAR(100) | Encrypted password             |
| photo    | VARCHAR(1000)| User avatar URL                |
| rating   | INT          | Player skill rating (ELO-based)|

#### Bots Table
| Field       | Type         | Description                    |
|-------------|--------------|--------------------------------|
| id          | INT          | Primary key, auto increment    |
| user_id     | INT          | Foreign key to Users table     |
| title       | VARCHAR(100) | Bot name/title                 |
| description | VARCHAR(300) | Bot description                |
| content     | TEXT         | Bot AI code (Java)             |
| create_time | DATETIME     | Bot creation timestamp         |
| modify_time | DATETIME     | Last modification timestamp    |

#### Records Table
| Field    | Type         | Description                    |
|----------|--------------|--------------------------------|
| id       | INT          | Primary key, auto increment    |
| a_id     | INT          | Player A user ID               |
| a_sx     | INT          | Player A starting X position   |
| a_sy     | INT          | Player A starting Y position   |
| b_id     | INT          | Player B user ID               |
| b_sx     | INT          | Player B starting X position   |
| b_sy     | INT          | Player B starting Y position   |
| a_steps  | TEXT         | Player A move sequence         |
| b_steps  | TEXT         | Player B move sequence         |
| map      | TEXT         | Game map layout                |
| loser    | VARCHAR(10)  | Game result (A/B/all for draw) |
| createtime| DATETIME    | Match completion timestamp     |

## Installation

### Prerequisites
- Java 8+
- Node.js 14+
- MySQL 8.0+
- Maven 3.6+

### Backend Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/King-of-Bots.git
   cd King-of-Bots
   ```

2. **Configure database**
   ```sql
   CREATE DATABASE kob;
   ```
   Update `backendcloud/backend/src/main/resources/application.properties` with your database credentials.

3. **Start the services**
   ```bash
   # Main backend service (port 3000)
   cd backendcloud/backend
   mvn spring-boot:run
   
   # Matching system (port 3001)
   cd backendcloud/matchingsystem
   mvn spring-boot:run
   
   # Bot running system (port 3002)
   cd backendcloud/botrunningsystem
   mvn spring-boot:run
   ```

### Frontend Setup

1. **Install dependencies**
   ```bash
   cd web
   npm install
   ```

2. **Start development server**
   ```bash
   npm run serve
   ```

3. **Access the application**
   Open http://localhost:8080 in your browser

## Usage

### Getting Started
1. **Register an account** or log in with existing credentials
2. **Explore the interface**: Navigate between battles, records, and rankings
3. **Start a match**: Click "Start Game" to join the matchmaking queue
4. **Control your snake**: Use arrow keys or WASD to move during battles

### Creating AI Bots
1. Navigate to "My Bots" section
2. Click "Create New Bot"
3. Write your AI logic in Java
4. Test and save your bot
5. Select your bot for automatic battles

### Game Rules
- Snakes grow longer over time
- Avoid hitting walls, obstacles, or other snakes
- Last snake standing wins
- Rating changes based on match results

## API Documentation

### Authentication Endpoints
| Method | Endpoint                         | Description        |
|--------|----------------------------------|--------------------|
| POST   | `/api/user/account/register/`    | User registration  |
| POST   | `/api/user/account/token/`       | User login         |
| GET    | `/api/user/account/info/`        | Get user info      |

### Game Endpoints
| Method | Endpoint                  | Description         |
|--------|---------------------------|---------------------|
| POST   | `/pk/start/game/`         | Start a new game    |
| POST   | `/pk/receive/bot/move/`   | Receive bot moves   |
| GET    | `/api/record/getlist/`    | Get match history   |
| GET    | `/api/ranklist/getlist/`  | Get leaderboard     |

### Bot Management
| Method | Endpoint                     | Description     |
|--------|------------------------------|-----------------|
| GET    | `/api/user/bot/getlist/`     | Get user bots   |
| POST   | `/api/user/bot/add/`         | Create new bot  |
| POST   | `/api/user/bot/update/`      | Update bot      |
| POST   | `/api/user/bot/remove/`      | Delete bot      |


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built with [Spring Boot](https://spring.io/projects/spring-boot) and [Vue.js](https://vuejs.org/)
- Inspired by classic snake games
- Uses modern web development best practices
- Implements microservices architecture patterns

