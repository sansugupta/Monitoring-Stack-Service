```mermaid
graph TD
    %% === Style Definitions for a Dark Theme ===
    classDef client fill:#1d2d44,stroke:#3e5c76,color:#f0f6fc,stroke-width:2px;
    classDef ingress fill:#24a1c1,stroke:#0d1117,color:#fff,font-weight:bold;
    classDef external fill:#333,stroke:#555,color:#f0f6fc;
    
    classDef prometheus fill:#e6522c,stroke:#0d1117,color:#fff,font-weight:bold;
    classDef loki fill:#fca311,stroke:#0d1117,color:#fff,font-weight:bold;
    classDef grafana fill:#32cd32,stroke:#0d1117,color:#fff,font-weight:bold;
    classDef alertmanager fill:#b22222,stroke:#0d1117,color:#fff,font-weight:bold;
    classDef otel fill:#9370db,stroke:#0d1117,color:#fff,font-weight:bold;
    classDef slack fill:#4a154b,stroke:#e0d2e1,color:#fff,font-weight:bold;

    %% === Architecture Definition ===

    subgraph "🌐 External World"
        A["fa:fa-google Google AdX"]
    end

    subgraph "💻 Your EKS Cluster"
        B["fa:fa-network-wired Traefik Ingress"]
        
        subgraph "Application Suite"
            C{"fa:fa-server EKS Services & Pods"}
            D1["fa:fa-cube po-bidder"]
            D2["fa:fa-cube po-dev-bidder"]
            D3["fa:fa-cube po-eventimpression"]
            D4["fa:fa-calendar-alt CronJob: Daily Report"]
            D5["fa:fa-calendar-alt CronJob: Monthly Report"]
        end
        
        E(("fa:fa-database<br>Databases"))
    end

    subgraph "🚀 PROPOSED OBSERVABILITY STACK (in 'monitoring' namespace)"
        subgraph "Data Collection & Processing"
            J["fa:fa-sitemap OpenTelemetry<br>Collector"]
            F["fa:fa-chart-line Prometheus<br>(Metrics)"]
            G["fa:fa-file-alt Loki<br>(Logs)"]
        end

        subgraph "Visualization & Alerting"
            I["fa:fa-chart-pie Grafana<br>(Dashboards)"]
            H["fa:fa-bell Alertmanager"]
            K["fa:fa-slack Slack Alerts"]
        end
    end
    
    %% === Connections ===
    A --> B
    B --> C
    C --> D1 & D2 & D3 & D4 & D5
    D1 & D2 & D3 & D4 & D5 --> E

    %% Data Flow into the Monitoring Stack
    C -- "Pod & App Metrics" --> F
    E -- "Database Metrics" --> F
    C -- "Structured Logs" --> G
    
    %% Optional Tracing Flow (Dotted Line)
    C -.->|Traces (Optional)| J
    J -- "Exports Data" .-> F & G

    %% Alerting Flow (Thick Line)
    F ==>|Fires Alerts| H
    H --> K

    %% Visualization Flow
    I -- "Queries Metrics" --> F
    I -- "Queries Logs" --> G

    %% === Apply Styles ===
    class A external;
    class B ingress;
    class C,D1,D2,D3,D4,D5,E client;
    class F prometheus;
    class G loki;
    class H alertmanager;
    class I grafana;
    class J otel;
    class K slack;
```
