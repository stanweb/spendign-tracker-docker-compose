# Spending Tracker Application

This project contains the Docker Compose setup for the Spending Tracker application.

## Overview

The Spending Tracker application is a web-based tool to help you track your expenses and transaction costs.

**Features:**

*   **Categorize Spending:** Create custom spending categories like "Rent," "Transport," and "Food."
*   **Budgeting:** Set budgets for each category to manage your spending effectively.
*   **Transaction Management:**
    *   Input transactions manually.
    *   Import transactions from M-Pesa statements (PDF).
    *   Add transactions from raw text.
*   **AI-Powered Imports:** The application uses AI to parse and generate transactions from M-Pesa statements and raw text.
    *   **Note:** It is advisable to use small PDF files, as the AI service is on a free tier.
*   **Visualizations:** Get a visual representation of your spending compared to your budgets.
*   **Top Spenders:** View the top recipients of your money, either overall or broken down by category.

The application consists of a backend service built with Spring Boot and a frontend service built with Next.js.

## Services

The `docker-compose.yml` file defines the following services:

-   **backend**: The Spring Boot application that provides the API for the Spending Tracker.
    -   Image: `skamau123/spending-tracker-services:latest`
    -   Ports: `8080:8080`
    -   Volumes: `tracker_sqlite:/data` (This is used by default for the embedded SQLite database)
-   **frontend**: The Next.js application that provides the user interface for the Spending Tracker.
    -   Image: `skamau123/spending-tracker-frontend:latest`
    -   Ports: `3000:3000`
    -   Depends on: `backend`

## Prerequisites

-   Docker
-   Docker Compose

## How to Run

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/stanweb/spendign-tracker-docker-compose.git
    ```
2.  **Create a `.env` file:**
    Create a `.env` file in the root of the project and add the following environment variables:

    ### Frontend
    ```
    GROQ_API_KEY=<your_groq_api_key>
    NEXT_PUBLIC_BACKEND_API_BASE_URL=http://backend:8080/api
    GROQ_MODEL=moonshotai/kimi-k2-instruct-0905
    ```
    - You can get your `GROQ_API_KEY` from [https://console.groq.com/keys](https://console.groq.com/keys).
    - Other possible values for `GROQ_MODEL` include:
        - `openai/gpt-oss-20b` (GPT-OSS 20B)
        - `openai/gpt-oss-120b` (GPT-OSS 120B)
        - `openai/gpt-oss-safeguard-20b` (Safety GPT OSS 20B)
        - `meta-llama/llama-4-maverick-17b-128e-instruct` (Llama 4 Maverick)
        - `meta-llama/llama-4-scout-17b-16e-instruct` (Llama 4 Scout)

    ### Backend (Optional - for external MySQL database)
    By default, the backend uses an embedded SQLite database. If you want to use your own MySQL database, add the following variables to your `.env` file:
    ```
    SPRING_DATASOURCE_URL=jdbc:mysql://localhost:3306/spending_tracker
    SPRING_DATASOURCE_USERNAME=tracker_user
    SPRING_DATASOURCE_PASSWORD=StrongPassword123!
    ```

3.  **Run the application:**
    ```bash
    docker-compose up -d
    ```
4.  **Access the application:**
    -   Frontend: [http://localhost:3000](http://localhost:3000)
    -   Backend: [http://localhost:8080](http://localhost:8080)

## Volumes

-   `tracker_sqlite`: This volume is used to persist the SQLite database used by the backend service. This is not used if you are using an external MySQL database.
