
# System Architecture

## Data Flow

```mermaid
flowchart LR
    subgraph CNC["CNC Controller (FANUC 0i)"]
        M1[("Macro Variables\n#600-#612")]
        G1[("G-code O9000/O9001")]
    end
    
    subgraph RPi["Raspberry Pi (32-bit OS)"]
        S[("Serial Reader\n(pyserial)")]
        P[("OEE Calculator\n(Python)")]
        MQ[("MQTT Broker\n(mosquitto)")]
    end
    
    subgraph NR["Node-RED"]
        D[("Dashboard\n(Web UI)")]
    end
    
    subgraph Cloud["Optional Cloud"]
        DB[(InfluxDB)]
        G[Hostinger]
    end
    
    G1 -->|updates| M1
    M1 -->|RS232 read| S
    S --> P
    P -->|JSON via MQTT| MQ
    MQ --> D
    MQ --> DB
    DB --> G
