---
title: 'TravlrNode'
date: 2024-03-14
type: landing

design:
  spacing: "6rem"

sections:
  - block: hero
    content:
      title: Decentralized Travel Data Management
      text: TravlrNode is a revolutionary blockchain-based platform that puts travelers in control of their data while enabling secure sharing across the travel ecosystem 🌍
      primary_action:
        text: Get Started
        url: /docs/getting-started/
        icon: rocket-launch
      secondary_action:
        text: Read the docs
        url: /docs/
      announcement:
        text: "Announcing TravlrNode v0.1 Release"
        link:
          text: "Read more"
          url: "/blog/"
    design:
      spacing:
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
      css_class: "from-primary-500 to-primary-700"
      
  - block: features
    id: features
    content:
      title: Key Features
      text: TravlrNode revolutionizes travel data management with blockchain technology, P2P networking, and decentralized identity for a secure, efficient, and user-centric travel experience.
      items:
        - name: Multi-Blockchain Support
          icon: link
          description: Support for Ethereum, Sui, and REST API fallback for flexible integration options.
        - name: Decentralized Identity
          icon: user-circle
          description: Built-in DID management using Veramo for secure identity control.
        - name: P2P Data Sharing
          icon: share
          description: Direct, secure data sharing between travel industry participants.
        - name: Access Control
          icon: lock-closed
          description: Granular permission management with time-based access control.
        - name: Verifiable Credentials
          icon: document-check
          description: Issue and verify travel-related credentials securely.
        - name: Modular Architecture
          icon: cube
          description: Plugin-based design for easy customization and extension.

  - block: cta-card
    content:
      title: "Transform Your Travel Data Management"
      text: Join the revolution in travel data management with TravlrNode. Enable secure, efficient, and user-controlled data sharing across the travel ecosystem.
      button:
        text: Get Started
        url: /docs/setup/
    design:
      card:
        css_class: "bg-primary-700"
        css_style: ""
---