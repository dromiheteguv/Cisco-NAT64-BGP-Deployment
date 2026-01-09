# Cisco-NAT64-BGP-Deployment
"Proven Cisco IOS-XE NAT64 configuration using BGP and 100.64.0.0/16 pool for IPv6-only Autonomous Systems."
# Cisco NAT64 BGP Deployment Guide

This repository contains a working, real-world configuration for a Cisco NAT64 Stateful Gateway. It is specifically designed for IPv6-only Autonomous Systems that need to route traffic to the IPv4 Internet.

## The Logical Diagram (Critical Connectivity)

```mermaid
graph TD
    subgraph Clients ["Retea IPv6-Only (AS-uri clienti)"]
        A[Client IPv6] --- B[Router Client BGP]
    end

    B --- C

    subgraph Node ["Router NAT64 (Nodul Tau)"]
        C["Interfata IPv6: nat64 enable"]
        C --- D{Traducere NAT64}
        D --- E["Interfata IPv4: nat64 enable"]
    end

    subgraph Internet ["Internet IPv4 (Segmentul Problematic)"]
        E --- F[Gateway ISP / NAT44]
        F --- G((Internet IPv4))
    end
    
    B -- "Sesiune BGP: Anunt 64:ff9b::/96" -- C

    
    
Important: Configurația spre Internet (WAN)
Pentru ca NAT64 să funcționeze, interfața spre internet TREBUIE să aibă nat64 enable, chiar dacă este o interfață pur IPv4. Fără această linie, routerul nu va ști să trimită pachetele traduse către Gateway-ul ISP-ului.
Specificatii Tehnice:

    Pool IPv4: 100.64.0.0/16 (Esențial pentru a evita coliziunile și a asigura mii de sesiuni).
    DNS64: Folosiți 2001:4860:4860::64 pentru a permite clienților IPv6 să "vadă" destinațiile IPv4.
