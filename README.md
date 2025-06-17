<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Taxi Guide - Enhanced with Load Shedding Alerts</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        :root {
            --primary: #2563eb;
            --primary-dark: #1d4ed8;
            --secondary: #10b981;
            --driver: #8b5cf6;
            --association: #ec4899;
            --dark: #1e293b;
            --light: #f8fafc;
            --gray: #94a3b8;
            --local: #10b981;
            --long-distance: #3b82f6;
            --danger: #ef4444;
            --premium: #f59e0b;
            --offline: #f59e0b;
            --safety: #ef4444;
            --menu-bg: #0f172a;
            --menu-item-hover: #1e293b;
            --menu-border: #334155;
            --strike-alert: #f59e0b;
            --roadworks-alert: #3b82f6;
            --protest-alert: #ef4444;
            --loadshedding-alert: #8b5cf6;
        }
        
        body {
            background: linear-gradient(135deg, #1e3a8a, #0ea5e9);
            color: var(--dark);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            overflow: hidden;
            position: relative;
        }
        
        .app-container {
            width: 100%;
            max-width: 500px;
            background: white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            display: flex;
            flex-direction: column;
            height: 90vh;
            position: relative;
            transition: transform 0.3s ease;
        }
        
        /* Header Styles */
        header {
            background: var(--primary);
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: relative;
            z-index: 10;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .logo i {
            font-size: 24px;
        }
        
        .logo h1 {
            font-size: 22px;
            font-weight: 700;
        }
        
        .user-type-badge {
            background: var(--driver);
            color: white;
            font-size: 12px;
            padding: 3px 10px;
            border-radius: 20px;
            margin-left: 10px;
        }
        
        .association-badge {
            background: var(--association);
        }
        
        .menu-btn {
            background: none;
            border: none;
            color: white;
            font-size: 24px;
            cursor: pointer;
            z-index: 100;
        }
        
        /* New Menu Styles */
        .menu-backdrop {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            z-index: 900;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }
        
        .menu-backdrop.active {
            opacity: 1;
            visibility: visible;
        }
        
        .menu {
            position: fixed;
            top: 0;
            left: -300px;
            width: 300px;
            height: 100vh;
            background: var(--menu-bg);
            z-index: 1000;
            overflow-y: auto;
            transition: transform 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            box-shadow: 5px 0 15px rgba(0, 0, 0, 0.5);
            display: flex;
            flex-direction: column;
        }
        
        .menu.active {
            transform: translateX(300px);
        }
        
        .menu-header {
            padding: 30px 20px 20px;
            border-bottom: 1px solid var(--menu-border);
            background: rgba(15, 23, 42, 0.9);
            position: relative;
        }
        
        .close-menu-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background: none;
            border: none;
            color: #94a3b8;
            font-size: 20px;
            cursor: pointer;
            transition: color 0.2s;
        }
        
        .close-menu-btn:hover {
            color: white;
        }
        
        .user-profile {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .user-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: linear-gradient(45deg, var(--primary), var(--primary-dark));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: white;
        }
        
        .driver-avatar {
            background: linear-gradient(45deg, var(--driver), #7c3aed);
        }
        
        .association-avatar {
            background: linear-gradient(45deg, var(--association), #db2777);
        }
        
        .user-info h3 {
            color: white;
            font-size: 18px;
            margin-bottom: 5px;
        }
        
        .user-info p {
            color: #94a3b8;
            font-size: 14px;
        }
        
        .premium-status {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(245, 158, 11, 0.15);
            border-radius: 10px;
            padding: 10px 15px;
            color: #f59e0b;
            font-size: 14px;
            margin-top: 10px;
        }
        
        .premium-status i {
            font-size: 18px;
        }
        
        .premium-status .btn-premium {
            background: rgba(245, 158, 11, 0.3);
            color: #f59e0b;
            border: none;
            border-radius: 20px;
            padding: 5px 15px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .premium-status .btn-premium:hover {
            background: rgba(245, 158, 11, 0.5);
        }
        
        .menu-items {
            padding: 20px 0;
            flex: 1;
        }
        
        .menu-item {
            display: flex;
            align-items: center;
            padding: 15px 25px;
            color: #cbd5e1;
            text-decoration: none;
            transition: all 0.2s;
            border-left: 4px solid transparent;
            cursor: pointer;
        }
        
        .menu-item:hover, .menu-item.active {
            background: var(--menu-item-hover);
            color: white;
            border-left: 4px solid var(--primary);
        }
        
        .driver-item:hover, .driver-item.active {
            border-left: 4px solid var(--driver);
        }
        
        .association-item:hover, .association-item.active {
            border-left: 4px solid var(--association);
        }
        
        .menu-item i {
            width: 30px;
            font-size: 18px;
            color: var(--primary);
        }
        
        .driver-item i {
            color: var(--driver);
        }
        
        .association-item i {
            color: var(--association);
        }
        
        .menu-item span {
            font-size: 16px;
            font-weight: 500;
        }
        
        .menu-footer {
            padding: 20px;
            border-top: 1px solid var(--menu-border);
            color: #94a3b8;
            font-size: 12px;
            text-align: center;
        }
        
        .menu-footer a {
            color: var(--primary);
            text-decoration: none;
        }
        
        /* Main Content */
        .main-content {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            position: relative;
        }
        
        .section-title {
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .section-title h2 {
            font-size: 24px;
            color: var(--dark);
        }
        
        .section-title p {
            color: var(--gray);
        }
        
        /* Map Container */
        .map-container {
            height: 300px;
            background: #e2e8f0;
            border-radius: 15px;
            margin-bottom: 25px;
            position: relative;
            overflow: hidden;
        }
        
        .map-overlay {
            position: absolute;
            top: 10px;
            left: 10px;
            right: 10px;
            background: rgba(255, 255, 255, 0.9);
            padding: 10px 15px;
            border-radius: 10px;
            display: flex;
            justify-content: space-between;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        
        .map-controls {
            display: flex;
            gap: 10px;
        }
        
        .map-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: white;
            border: 1px solid #cbd5e1;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 18px;
        }
        
        /* Taxi Markers */
        .taxi-marker {
            position: absolute;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            color: white;
            font-weight: bold;
            transform: translate(-50%, -50%);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
            z-index: 2;
            transition: all 0.5s ease;
        }
        
        .local-taxi {
            background: var(--local);
            border: 2px solid white;
        }
        
        .long-distance-taxi {
            background: var(--long-distance);
            border: 2px solid white;
        }
        
        .verified-taxi::after {
            content: "";
            position: absolute;
            bottom: -5px;
            right: -5px;
            width: 15px;
            height: 15px;
            background: var(--premium);
            border-radius: 50%;
            border: 2px solid white;
        }
        
        .user-marker {
            position: absolute;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--danger);
            border: 2px solid white;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
            z-index: 3;
        }
        
        /* Taxi List */
        .taxi-list {
            display: grid;
            gap: 15px;
        }
        
        .taxi-card {
            background: white;
            border-radius: 15px;
            padding: 15px;
            display: flex;
            align-items: center;
            gap: 15px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
            border-left: 4px solid var(--local);
            transition: transform 0.2s;
            position: relative;
        }
        
        .taxi-card:hover {
            transform: translateY(-3px);
        }
        
        .taxi-card.long-distance {
            border-left-color: var(--long-distance);
        }
        
        .taxi-icon {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            color: white;
        }
        
        .taxi-card.local .taxi-icon {
            background: var(--local);
        }
        
        .taxi-card.long-distance .taxi-icon {
            background: var(--long-distance);
        }
        
        .taxi-info {
            flex: 1;
        }
        
        .taxi-info h3 {
            font-size: 18px;
            margin-bottom: 5px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .taxi-type {
            font-size: 12px;
            padding: 2px 8px;
            border-radius: 20px;
            background: #e2e8f0;
        }
        
        .taxi-meta {
            display: flex;
            gap: 15px;
            font-size: 14px;
            color: var(--gray);
        }
        
        .taxi-meta span {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .taxi-eta {
            background: var(--primary);
            color: white;
            padding: 5px 12px;
            border-radius: 20px;
            font-weight: 600;
            font-size: 14px;
        }
        
        .premium-badge {
            position: absolute;
            top: 10px;
            right: 10px;
            background: var(--premium);
            color: white;
            font-size: 10px;
            padding: 2px 8px;
            border-radius: 20px;
            font-weight: bold;
        }
        
        /* Associations */
        .association-list {
            display: grid;
            gap: 15px;
        }
        
        .association-card {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
        }
        
        .association-logo {
            width: 50px;
            height: 50px;
            border-radius: 12px;
            background: linear-gradient(45deg, var(--primary), var(--primary-dark));
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            font-size: 20px;
        }
        
        .association-info {
            flex: 1;
        }
        
        .association-info h3 {
            margin-bottom: 5px;
        }
        
        .association-stats {
            display: flex;
            gap: 15px;
            font-size: 14px;
            color: var(--gray);
        }
        
        /* Bottom Navigation */
        .bottom-nav {
            display: flex;
            background: white;
            border-top: 1px solid #e2e8f0;
        }
        
        .nav-item {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 12px;
            color: var(--gray);
            font-size: 12px;
            transition: all 0.2s;
            cursor: pointer;
        }
        
        .nav-item.active {
            color: var(--primary);
        }
        
        .driver-nav.active {
            color: var(--driver);
        }
        
        .association-nav.active {
            color: var(--association);
        }
        
        .nav-item i {
            font-size: 20px;
            margin-bottom: 5px;
        }
        
        /* Auth Screen */
        .auth-container {
            padding: 30px 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .auth-logo {
            width: 80px;
            height: 80px;
            background: linear-gradient(45deg, var(--primary), var(--primary-dark));
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 40px;
            margin-bottom: 20px;
        }
        
        .auth-title {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .auth-title h2 {
            font-size: 28px;
            margin-bottom: 10px;
            color: var(--dark);
        }
        
        .auth-title p {
            color: var(--gray);
        }
        
        .auth-form {
            width: 100%;
            max-width: 400px;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
            color: var(--dark);
        }
        
        .form-control {
            width: 100%;
            padding: 15px;
            border: 1px solid #cbd5e1;
            border-radius: 10px;
            font-size: 16px;
            transition: border 0.3s;
        }
        
        .form-control:focus {
            outline: none;
            border-color: var(--primary);
        }
        
        .btn {
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .btn-primary {
            background: var(--primary);
            color: white;
        }
        
        .btn-primary:hover {
            background: var(--primary-dark);
        }
        
        .btn-driver {
            background: var(--driver);
        }
        
        .btn-driver:hover {
            background: #7c3aed;
        }
        
        .btn-association {
            background: var(--association);
        }
        
        .btn-association:hover {
            background: #db2777;
        }
        
        .btn-outline {
            background: transparent;
            border: 1px solid var(--primary);
            color: var(--primary);
        }
        
        .btn-outline:hover {
            background: #e0e7ff;
        }
        
        .auth-footer {
            margin-top: 20px;
            text-align: center;
            color: var(--gray);
        }
        
        .auth-footer a {
            color: var(--primary);
            text-decoration: none;
            font-weight: 500;
        }
        
        .divider {
            display: flex;
            align-items: center;
            margin: 20px 0;
            color: var(--gray);
        }
        
        .divider::before,
        .divider::after {
            content: '';
            flex: 1;
            border-bottom: 1px solid #e2e8f0;
        }
        
        .divider span {
            padding: 0 15px;
        }
        
        /* User Type Selection */
        .user-type-selector {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        .user-type-btn {
            flex: 1;
            padding: 15px;
            background: #f1f5f9;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .user-type-btn.active {
            border-color: var(--primary);
            background: #dbeafe;
        }
        
        .user-type-btn.driver.active {
            border-color: var(--driver);
            background: #ede9fe;
        }
        
        .user-type-btn.association.active {
            border-color: var(--association);
            background: #fce7f3;
        }
        
        .user-type-btn i {
            font-size: 24px;
            margin-bottom: 10px;
            color: var(--gray);
        }
        
        .user-type-btn.active i {
            color: var(--primary);
        }
        
        .user-type-btn.driver.active i {
            color: var(--driver);
        }
        
        .user-type-btn.association.active i {
            color: var(--association);
        }
        
        /* Hidden screens */
        .screen {
            display: none;
        }
        
        .screen.active {
            display: block;
        }
        
        /* Dashboard Styles */
        .dashboard-card {
            background: white;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
            border-left: 4px solid var(--primary);
        }
        
        .driver-dashboard .dashboard-card {
            border-left: 4px solid var(--driver);
        }
        
        .association-dashboard .dashboard-card {
            border-left: 4px solid var(--association);
        }
        
        .dashboard-title {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        .dashboard-title i {
            font-size: 24px;
            color: var(--primary);
        }
        
        .driver-dashboard .dashboard-title i {
            color: var(--driver);
        }
        
        .association-dashboard .dashboard-title i {
            color: var(--association);
        }
        
        .dashboard-title h2 {
            font-size: 20px;
            color: var(--dark);
        }
        
        .form-row {
            display: flex;
            gap: 15px;
            margin-bottom: 15px;
        }
        
        .form-group {
            flex: 1;
        }
        
        .route-list {
            display: grid;
            gap: 10px;
        }
        
        .route-item {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            background: #f8fafc;
            border-radius: 10px;
            border-left: 3px solid var(--primary);
        }
        
        .route-item .actions {
            display: flex;
            gap: 10px;
        }
        
        .action-btn {
            background: none;
            border: none;
            color: var(--gray);
            cursor: pointer;
            font-size: 16px;
        }
        
        .action-btn:hover {
            color: var(--primary);
        }
        
        .add-route-btn {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 15px;
            color: var(--primary);
            background: none;
            border: none;
            font-weight: 500;
            cursor: pointer;
        }
        
        /* New Features */
        .offline-indicator {
            position: absolute;
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--offline);
            color: white;
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 12px;
            z-index: 100;
            display: none;
        }
        
        .emergency-btn {
            position: absolute;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: var(--safety);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            box-shadow: 0 4px 15px rgba(239, 68, 68, 0.4);
            z-index: 50;
            cursor: pointer;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
        
        .ad-container {
            background: #f1f5f9;
            border-radius: 10px;
            padding: 15px;
            margin: 15px 0;
            text-align: center;
            border: 1px dashed #cbd5e1;
        }
        
        .ad-label {
            font-size: 10px;
            color: var(--gray);
            text-transform: uppercase;
            margin-bottom: 5px;
        }
        
        .ad-content {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .ad-icon {
            width: 40px;
            height: 40px;
            background: var(--primary);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 20px;
        }
        
        .ad-text {
            flex: 1;
            text-align: left;
        }
        
        .ad-text h4 {
            font-size: 14px;
            margin-bottom: 3px;
            color: var(--dark);
        }
        
        .ad-text p {
            font-size: 12px;
            color: var(--gray);
        }
        
        .data-saver-toggle {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 15px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
            margin-bottom: 15px;
        }
        
        .toggle-switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 24px;
        }
        
        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 24px;
        }
        
        .slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .slider {
            background-color: var(--secondary);
        }
        
        input:checked + .slider:before {
            transform: translateX(26px);
        }
        
        .premium-offer {
            background: linear-gradient(45deg, #f59e0b, #fbbf24);
            color: white;
            padding: 15px;
            border-radius: 15px;
            text-align: center;
            margin: 15px 0;
        }
        
        .premium-offer h3 {
            margin-bottom: 10px;
            font-size: 18px;
        }
        
        .premium-offer p {
            margin-bottom: 15px;
            font-size: 14px;
        }
        
        .btn-premium {
            background: white;
            color: #f59e0b;
            font-weight: bold;
        }
        
        .settings-screen .taxi-card {
            border-left: 4px solid var(--primary);
        }
        
        .driver-dashboard .nav-item.driver-nav.active {
            color: var(--driver);
        }
        
        .association-dashboard .nav-item.association-nav.active {
            color: var(--association);
        }
        
        /* NEW ALERT STYLES */
        .alerts-container {
            margin: 15px 0;
            display: grid;
            gap: 10px;
        }
        
        .alert-item {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 12px 15px;
            border-radius: 10px;
            font-size: 14px;
            color: white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .alert-item i {
            font-size: 18px;
        }
        
        .alert-content {
            flex: 1;
        }
        
        .alert-title {
            font-weight: 600;
            margin-bottom: 3px;
        }
        
        .alert-description {
            font-size: 13px;
            opacity: 0.9;
        }
        
        .close-alert {
            background: none;
            border: none;
            color: white;
            opacity: 0.7;
            cursor: pointer;
            padding: 5px;
            font-size: 14px;
        }
        
        .strike-alert {
            background: var(--strike-alert);
        }
        
        .roadworks-alert {
            background: var(--roadworks-alert);
        }
        
        .protest-alert {
            background: var(--protest-alert);
        }
        
        .loadshedding-alert {
            background: var(--loadshedding-alert);
        }
        
        .alert-indicator {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 10px;
            height: 10px;
            border-radius: 50%;
        }
        
        .map-alert {
            position: absolute;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            color: white;
            font-weight: bold;
            transform: translate(-50%, -50%);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
            z-index: 4;
            animation: pulse 1.5s infinite;
        }
        
        .strike-map-alert {
            background: var(--strike-alert);
        }
        
        .roadworks-map-alert {
            background: var(--roadworks-alert);
        }
        
        .protest-map-alert {
            background: var(--protest-alert);
        }
        
        .loadshedding-map-alert {
            background: var(--loadshedding-alert);
        }
        
        .alert-map-icon {
            position: absolute;
            width: 24px;
            height: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            color: white;
            border-radius: 50%;
            transform: translate(-50%, -50%);
            z-index: 4;
        }
        
        .alerts-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin: 15px 0 10px;
        }
        
        .alerts-title {
            font-size: 18px;
            color: var(--dark);
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .clear-alerts {
            background: none;
            border: none;
            color: var(--primary);
            font-size: 13px;
            font-weight: 500;
            cursor: pointer;
        }
        
        .no-alerts {
            text-align: center;
            padding: 20px;
            color: var(--gray);
            font-size: 14px;
        }
        
        /* Load Shedding Styles */
        .loadshedding-indicator {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 24px;
            height: 24px;
            border-radius: 50%;
            background: var(--loadshedding-alert);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 12px;
        }
        
        .loadshedding-info {
            display: flex;
            align-items: center;
            gap: 5px;
            background: rgba(139, 92, 246, 0.1);
            border-radius: 20px;
            padding: 5px 10px;
            margin-top: 5px;
            font-size: 12px;
            color: var(--loadshedding-alert);
        }
        
        .loadshedding-schedule {
            background: rgba(139, 92, 246, 0.1);
            border-radius: 10px;
            padding: 8px 12px;
            font-size: 12px;
            color: var(--loadshedding-alert);
            margin-top: 5px;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .dashboard-alert {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 10px;
            border-radius: 10px;
            margin-bottom: 15px;
            color: white;
        }
        
        .driver-alert {
            background: var(--driver);
        }
        
        .association-alert {
            background: var(--association);
        }
        
        .alert-badge {
            position: absolute;
            top: -5px;
            right: -5px;
            background: var(--danger);
            color: white;
            font-size: 10px;
            width: 18px;
            height: 18px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    </style>
</head>
<body>
    <div class="app-container">
        <!-- Menu Backdrop -->
        <div class="menu-backdrop" id="menuBackdrop"></div>
        
        <!-- Dark Theme Menu -->
        <div class="menu" id="menu">
            <div class="menu-header">
                <button class="close-menu-btn" id="closeMenuBtn">
                    <i class="fas fa-times"></i>
                </button>
                <div class="user-profile">
                    <div class="user-avatar" id="userAvatar">
                        <i class="fas fa-user"></i>
                    </div>
                    <div class="user-info">
                        <h3 id="menuUserName">Martin Memela</h3>
                        <p id="menuUserEmail">memelasfisomartin@gmail.com</p>
                    </div>
                </div>
                <div class="premium-status">
                    <div>
                        <i class="fas fa-crown"></i>
                        <span>Premium Status</span>
                    </div>
                    <button class="btn-premium">Upgrade</button>
                </div>
            </div>
            
            <div class="menu-items">
                <a class="menu-item active" data-target="mapScreen">
                    <i class="fas fa-map"></i>
                    <span>Live Map</span>
                </a>
                <a class="menu-item" data-target="associationsScreen">
                    <i class="fas fa-building"></i>
                    <span>Associations</span>
                </a>
                <a class="menu-item" data-target="routesScreen">
                    <i class="fas fa-route"></i>
                    <span>Routes</span>
                </a>
                <a class="menu-item driver-item" data-target="driverDashboard" id="driverDashboardMenuItem">
                    <i class="fas fa-taxi"></i>
                    <span>Driver Dashboard</span>
                </a>
                <a class="menu-item association-item" data-target="associationDashboard" id="associationDashboardMenuItem">
                    <i class="fas fa-users"></i>
                    <span>Association Dashboard</span>
                </a>
                <a class="menu-item" data-target="profileScreen">
                    <i class="fas fa-user"></i>
                    <span>My Profile</span>
                </a>
                <a class="menu-item" data-target="settingsScreen">
                    <i class="fas fa-cog"></i>
                    <span>Settings</span>
                </a>
                <a class="menu-item">
                    <i class="fas fa-history"></i>
                    <span>Ride History</span>
                </a>
                <a class="menu-item">
                    <i class="fas fa-wallet"></i>
                    <span>Payments</span>
                </a>
                <a class="menu-item">
                    <i class="fas fa-shield-alt"></i>
                    <span>Safety Center</span>
                </a>
                <a class="menu-item">
                    <i class="fas fa-question-circle"></i>
                    <span>Help & Support</span>
                </a>
            </div>
            
            <div class="menu-footer">
                <p>Taxi Guide v2.1.5</p>
                <p>© 2023 Taxi Guide Inc. <a href="#">Privacy Policy</a></p>
            </div>
        </div>
        
        <!-- Offline Indicator -->
        <div class="offline-indicator" id="offlineIndicator">
            <i class="fas fa-wifi"></i> Offline Mode
        </div>
        
        <!-- Emergency Button -->
        <div class="emergency-btn" id="emergencyBtn">
            <i class="fas fa-shield-alt"></i>
        </div>
        
        <!-- Auth Screen -->
        <div class="auth-container screen active" id="authScreen">
            <div class="auth-logo">
                <i class="fas fa-taxi"></i>
            </div>
            <div class="auth-title">
                <h2>Welcome to Taxi Guide</h2>
                <p>Track minibus taxis with real-time alerts</p>
            </div>
            
            <div class="auth-form" id="loginForm">
                <div class="user-type-selector" id="userTypeSelector">
                    <div class="user-type-btn" data-type="passenger">
                        <i class="fas fa-user"></i>
                        <div>Passenger</div>
                    </div>
                    <div class="user-type-btn driver" data-type="driver">
                        <i class="fas fa-taxi"></i>
                        <div>Driver</div>
                    </div>
                    <div class="user-type-btn association" data-type="association">
                        <i class="fas fa-users"></i>
                        <div>Association</div>
                    </div>
                </div>
                
                <div class="form-group">
                    <label for="email">Email</label>
                    <input type="email" id="email" class="form-control" placeholder="Enter your email">
                </div>
                
                <div class="form-group">
                    <label for="password">Password</label>
                    <input type="password" id="password" class="form-control" placeholder="Enter your password">
                </div>
                
                <button class="btn btn-primary" id="loginBtn">Login</button>
                
                <div class="divider">
                    <span>or</span>
                </div>
                
                <button class="btn btn-outline" id="signupBtn">Create New Account</button>
                
                <div class="auth-footer">
                    <p>By continuing, you agree to our <a href="#">Terms of Service</a> and <a href="#">Privacy Policy</a></p>
                </div>
            </div>
        </div>
        
        <!-- Main App (hidden until login) -->
        <div class="screen" id="mainApp">
            <header>
                <div class="logo">
                    <i class="fas fa-taxi"></i>
                    <h1>Taxi Guide <span class="user-type-badge" id="userTypeBadge">Passenger</span></h1>
                </div>
                <button class="menu-btn" id="menuBtn">
                    <i class="fas fa-bars"></i>
                </button>
            </header>
            
            <div class="main-content">
                <!-- Map Screen -->
                <div class="screen active" id="mapScreen">
                    <div class="section-title">
                        <div>
                            <h2>Live Taxi Map</h2>
                            <p>Real-time tracking with alerts</p>
                        </div>
                    </div>
                    
                    <div class="map-container" id="map">
                        <div class="map-overlay">
                            <div>Johannesburg</div>
                            <div class="map-controls">
                                <button class="map-btn" id="locateBtn">
                                    <i class="fas fa-location-arrow"></i>
                                </button>
                                <button class="map-btn">
                                    <i class="fas fa-search"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                    
                    <!-- ALERTS SECTION -->
                    <div class="alerts-header">
                        <h3 class="alerts-title"><i class="fas fa-exclamation-triangle"></i> Active Alerts</h3>
                        <button class="clear-alerts" id="clearAlertsBtn">Clear All</button>
                    </div>
                    
                    <div class="alerts-container" id="alertsContainer">
                        <!-- Alerts will be dynamically added here -->
                        <div class="no-alerts" id="noAlertsMessage">
                            <i class="fas fa-check-circle"></i>
                            <p>No active alerts in your area</p>
                        </div>
                    </div>
                    
                    <div class="section-title">
                        <h2>Nearby Taxis</h2>
                        <p>ETA to your location</p>
                    </div>
                    
                    <div class="taxi-list" id="taxiList">
                        <!-- Taxi cards will be added here -->
                    </div>
                </div>
                
                <!-- Associations Screen -->
                <div class="screen" id="associationsScreen">
                    <div class="section-title">
                        <div>
                            <h2>Taxi Associations</h2>
                            <p>1200 registered associations</p>
                        </div>
                    </div>
                    
                    <div class="association-list" id="associationList">
                        <!-- Associations will be added here -->
                    </div>
                </div>
                
                <!-- Routes Screen -->
                <div class="screen" id="routesScreen">
                    <div class="section-title">
                        <div>
                            <h2>Taxi Routes</h2>
                            <p>Daily routes and schedules</p>
                        </div>
                    </div>
                    
                    <div class="taxi-list">
                        <div class="taxi-card local">
                            <div class="taxi-icon">
                                <i class="fas fa-bus"></i>
                            </div>
                            <div class="taxi-info">
                                <h3>City Center - North Suburbs <span class="taxi-type">Local</span></h3>
                                <div class="taxi-meta">
                                    <span><i class="fas fa-road"></i> 12km</span>
                                    <span><i class="fas fa-clock"></i> 25 min</span>
                                    <span><i class="fas fa-money-bill-wave"></i> R15.00</span>
                                </div>
                                <div class="loadshedding-info">
                                    <i class="fas fa-bolt"></i>
                                    <span>Load Shedding: Stage 4 (06:00-08:30, 16:00-18:30)</span>
                                </div>
                            </div>
                        </div>
                        
                        <div class="taxi-card long-distance">
                            <div class="taxi-icon">
                                <i class="fas fa-bus"></i>
                            </div>
                            <div class="taxi-info">
                                <h3>Johannesburg - Pretoria <span class="taxi-type">Long Distance</span></h3>
                                <div class="taxi-meta">
                                    <span><i class="fas fa-road"></i> 58km</span>
                                    <span><i class="fas fa-clock"></i> 60 min</span>
                                    <span><i class="fas fa-money-bill-wave"></i> R45.00</span>
                                </div>
                                <div class="loadshedding-info">
                                    <i class="fas fa-bolt"></i>
                                    <span>Load Shedding: Stage 2 (10:00-12:30)</span>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Ad Container -->
                        <div class="ad-container">
                            <div class="ad-label">Advertisement</div>
                            <div class="ad-content">
                                <div class="ad-icon">
                                    <i class="fas fa-mobile-alt"></i>
                                </div>
                                <div class="ad-text">
                                    <h4>Get 500MB Free Data</h4>
                                    <p>Vodacom users: Dial *123# for free data with Taxi Guide</p>
                                </div>
                            </div>
                        </div>
                        
                        <div class="taxi-card local">
                            <div class="taxi-icon">
                                <i class="fas fa-bus"></i>
                            </div>
                            <div class="taxi-info">
                                <h3>Soweto - CBD <span class="taxi-type">Local</span></h3>
                                <div class="taxi-meta">
                                    <span><i class="fas fa-road"></i> 18km</span>
                                    <span><i class="fas fa-clock"></i> 35 min</span>
                                    <span><i class="fas fa-money-bill-wave"></i> R20.00</span>
                                </div>
                                <div class="loadshedding-info">
                                    <i class="fas fa-bolt"></i>
                                    <span>Load Shedding: Stage 4 (14:00-16:30)</span>
                                </div>
                            </div>
                        </div>
                        
                        <div class="taxi-card long-distance">
                            <div class="taxi-icon">
                                <i class="fas fa-bus"></i>
                            </div>
                            <div class="taxi-info">
                                <h3>Johannesburg - Durban <span class="taxi-type">Long Distance</span></h3>
                                <div class="taxi-meta">
                                    <span><i class="fas fa-road"></i> 568km</span>
                                    <span><i class="fas fa-clock"></i> 6 hours</span>
                                    <span><i class="fas fa-money-bill-wave"></i> R250.00</span>
                                </div>
                                <div class="loadshedding-info">
                                    <i class="fas fa-bolt"></i>
                                    <span>Load Shedding: Stage 3 (18:00-20:30)</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Driver Dashboard -->
                <div class="screen driver-dashboard" id="driverDashboard">
                    <div class="dashboard-alert driver-alert">
                        <i class="fas fa-exclamation-triangle"></i>
                        <div>Strike alert: Taxi association strike planned tomorrow from 8am-12pm</div>
                    </div>
                    
                    <div class="dashboard-card">
                        <div class="dashboard-title">
                            <i class="fas fa-taxi"></i>
                            <h2>Driver Profile</h2>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label>Full Name</label>
                                <input type="text" class="form-control" value="Thabo Mbeki">
                            </div>
                            <div class="form-group">
                                <label>Taxi Registration</label>
                                <input type="text" class="form-control" value="ABC 123 GP">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label>Phone Number</label>
                                <input type="tel" class="form-control" value="+27 82 123 4567">
                            </div>
                            <div class="form-group">
                                <label>Taxi Type</label>
                                <select class="form-control">
                                    <option>Minibus (16-seater)</option>
                                    <option>Midibus (22-seater)</option>
                                    <option>Sedan</option>
                                </select>
                            </div>
                        </div>
                        
                        <div class="loadshedding-schedule">
                            <i class="fas fa-bolt"></i>
                            <span>Your area: Stage 4 Load Shedding (06:00-08:30, 16:00-18:30)</span>
                        </div>
                        
                        <button class="btn btn-driver">Save Changes</button>
                    </div>
                    
                    <div class="dashboard-card">
                        <div class="dashboard-title">
                            <i class="fas fa-route"></i>
                            <h2>My Routes & Pricing</h2>
                        </div>
                        
                        <div class="route-list">
                            <div class="route-item">
                                <div>
                                    <strong>Soweto to Johannesburg CBD</strong>
                                    <div>R20.00 (Local)</div>
                                    <div class="loadshedding-info">
                                        <i class="fas fa-bolt"></i>
                                        <span>Stage 4 (06:00-08:30, 16:00-18:30)</span>
                                    </div>
                                </div>
                                <div class="actions">
                                    <button class="action-btn"><i class="fas fa-edit"></i></button>
                                    <button class="action-btn"><i class="fas fa-trash"></i></button>
                                </div>
                            </div>
                            
                            <div class="route-item">
                                <div>
                                    <strong>Alexandra to Sandton</strong>
                                    <div>R15.00 (Local)</div>
                                    <div class="loadshedding-info">
                                        <i class="fas fa-bolt"></i>
                                        <span>Stage 3 (14:00-16:30)</span>
                                    </div>
                                </div>
                                <div class="actions">
                                    <button class="action-btn"><i class="fas fa-edit"></i></button>
                                    <button class="action-btn"><i class="fas fa-trash"></i></button>
                                </div>
                            </div>
                        </div>
                        
                        <button class="add-route-btn">
                            <i class="fas fa-plus-circle"></i> Add New Route
                        </button>
                    </div>
                    
                    <div class="dashboard-card">
                        <div class="dashboard-title">
                            <i class="fas fa-chart-line"></i>
                            <h2>Performance Stats</h2>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label>Daily Trips</label>
                                <input type="text" class="form-control" value="8" readonly>
                            </div>
                            <div class="form-group">
                                <label>Average Rating</label>
                                <input type="text" class="form-control" value="4.7 ★" readonly>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label>Weekly Earnings</label>
                            <input type="text" class="form-control" value="R8,450" readonly>
                        </div>
                    </div>
                </div>
                
                <!-- Association Dashboard -->
                <div class="screen association-dashboard" id="associationDashboard">
                    <div class="dashboard-alert association-alert">
                        <i class="fas fa-exclamation-triangle"></i>
                        <div>Protest alert: Road closures in Soweto until 5pm today</div>
                    </div>
                    
                    <div class="dashboard-card">
                        <div class="dashboard-title">
                            <i class="fas fa-users"></i>
                            <h2>Association Profile</h2>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label>Association Name</label>
                                <input type="text" class="form-control" value="Metro Taxis Association">
                            </div>
                            <div class="form-group">
                                <label>Registration Number</label>
                                <input type="text" class="form-control" value="TAX123456SA">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label>Contact Person</label>
                                <input type="text" class="form-control" value="Mandla Nkosi">
                            </div>
                            <div class="form-group">
                                <label>Contact Number</label>
                                <input type="tel" class="form-control" value="+27 11 123 4567">
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label>Operational Regions</label>
                            <input type="text" class="form-control" value="Johannesburg CBD, Soweto, Alexandra">
                        </div>
                        
                        <div class="loadshedding-schedule">
                            <i class="fas fa-bolt"></i>
                            <span>Association-wide: Stage 4 Load Shedding (06:00-08:30, 16:00-18:30)</span>
                        </div>
                        
                        <button class="btn btn-association">Save Changes</button>
                    </div>
                    
                    <div class="dashboard-card">
                        <div class="dashboard-title">
                            <i class="fas fa-route"></i>
                            <h2>Association Routes & Pricing</h2>
                        </div>
                        
                        <div class="route-list">
                            <div class="route-item">
                                <div>
                                    <strong>Soweto to Johannesburg CBD</strong>
                                    <div>R20.00 (Standard)</div>
                                    <div class="loadshedding-info">
                                        <i class="fas fa-bolt"></i>
                                        <span>Stage 4 (06:00-08:30, 16:00-18:30)</span>
                                    </div>
                                </div>
                                <div class="actions">
                                    <button class="action-btn"><i class="fas fa-edit"></i></button>
                                    <button class="action-btn"><i class="fas fa-trash"></i></button>
                                </div>
                            </div>
                            
                            <div class="route-item">
                                <div>
                                    <strong>Johannesburg to Pretoria</strong>
                                    <div>R45.00 (Long Distance)</div>
                                    <div class="loadshedding-info">
                                        <i class="fas fa-bolt"></i>
                                        <span>Stage 2 (10:00-12:30)</span>
                                    </div>
                                </div>
                                <div class="actions">
                                    <button class="action-btn"><i class="fas fa-edit"></i></button>
                                    <button class="action-btn"><i class="fas fa-trash"></i></button>
                                </div>
                            </div>
                            
                            <div class="route-item">
                                <div>
                                    <strong>Alexandra to Sandton</strong>
                                    <div>R15.00 (Standard)</div>
                                    <div class="loadshedding-info">
                                        <i class="fas fa-bolt"></i>
                                        <span>Stage 4 (14:00-16:30)</span>
                                    </div>
                                </div>
                                <div class="actions">
                                    <button class="action-btn"><i class="fas fa-edit"></i></button>
                                    <button class="action-btn"><i class="fas fa-trash"></i></button>
                                </div>
                            </div>
                        </div>
                        
                        <button class="add-route-btn">
                            <i class="fas fa-plus-circle"></i> Add New Route
                        </button>
                    </div>
                    
                    <div class="dashboard-card">
                        <div class="dashboard-title">
                            <i class="fas fa-taxi"></i>
                            <h2>Member Management</h2>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label>Total Drivers</label>
                                <input type="text" class="form-control" value="250" readonly>
                            </div>
                            <div class="form-group">
                                <label>Active Today</label>
                                <input type="text" class="form-control" value="187" readonly>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label>Add New Driver</label>
                            <div class="form-row">
                                <input type="text" class="form-control" placeholder="Driver Name">
                                <button class="btn btn-association" style="width: auto;">Add</button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Profile Screen -->
                <div class="screen" id="profileScreen">
                    <div style="text-align: center; padding: 20px 0;">
                        <div style="width: 100px; height: 100px; border-radius: 50%; background: linear-gradient(45deg, var(--primary), var(--primary-dark)); margin: 0 auto 20px; display: flex; align-items: center; justify-content: center;">
                            <i class="fas fa-user" style="color: white; font-size: 40px;"></i>
                        </div>
                        <h2>Martin Memela</h2>
                        <p>memelasfisomartin@gmail.com</p>
                        <p>+27 699308804 </p>
                        
                        <!-- Load Shedding Info -->
                        <div class="loadshedding-schedule" style="margin: 15px auto; max-width: 300px;">
                            <i class="fas fa-bolt"></i>
                            <span>Your area: Stage 4 Load Shedding (06:00-08:30, 16:00-18:30)</span>
                        </div>
                        
                        <!-- Premium Status -->
                        <div class="premium-offer" id="premiumOffer">
                            <h3>Upgrade to Premium</h3>
                            <p>Get verified taxis, no ads, and priority support</p>
                            <button class="btn btn-premium" id="upgradeBtn">
                                <i class="fas fa-crown"></i> R49/month
                            </button>
                        </div>
                    </div>
                    
                    <div class="taxi-list">
                        <div class="taxi-card">
                            <div style="flex: 1;">
                                <h3><i class="fas fa-user-edit" style="margin-right: 10px;"></i> Edit Profile</h3>
                            </div>
                            <i class="fas fa-chevron-right"></i>
                        </div>
                        
                        <div class="taxi-card">
                            <div style="flex: 1;">
                                <h3><i class="fas fa-credit-card" style="margin-right: 10px;"></i> Payment Methods</h3>
                            </div>
                            <i class="fas fa-chevron-right"></i>
                        </div>
                        
                        <div class="taxi-card">
                            <div style="flex: 1;">
                                <h3><i class="fas fa-history" style="margin-right: 10px;"></i> Ride History</h3>
                            </div>
                            <i class="fas fa-chevron-right"></i>
                        </div>
                        
                        <div class="taxi-card">
                            <div style="flex: 1;">
                                <h3><i class="fas fa-cog" style="margin-right: 10px;"></i> Settings</h3>
                            </div>
                            <i class="fas fa-chevron-right"></i>
                        </div>
                        
                        <div class="taxi-card">
                            <div style="flex: 1;">
                                <h3><i class="fas fa-question-circle" style="margin-right: 10px;"></i> Help & Support</h3>
                            </div>
                            <i class="fas fa-chevron-right"></i>
                        </div>
                        
                        <div class="taxi-card" style="border-left-color: var(--danger);">
                            <div style="flex: 1;">
                                <h3><i class="fas fa-sign-out-alt" style="margin-right: 10px;"></i> Logout</h3>
                            </div>
                            <i class="fas fa-chevron-right"></i>
                        </div>
                    </div>
                </div>
                
                <!-- Settings Screen -->
                <div class="screen settings-screen" id="settingsScreen">
                    <div class="section-title">
                        <h2>App Settings</h2>
                        <p>Customize your experience</p>
                    </div>
                    
                    <div class="data-saver-toggle">
                        <div>
                            <h3>Data Saver Mode</h3>
                            <p>Reduce data usage by limiting map details</p>
                        </div>
                        <label class="toggle-switch">
                            <input type="checkbox" id="dataSaverToggle">
                            <span class="slider"></span>
                        </label>
                    </div>
                    
                    <div class="taxi-card">
                        <div style="flex: 1;">
                            <h3><i class="fas fa-map-marked-alt" style="margin-right: 10px;"></i> Offline Maps</h3>
                            <p>Download maps for offline use</p>
                        </div>
                        <i class="fas fa-chevron-right"></i>
                    </div>
                    
                    <div class="taxi-card">
                        <div style="flex: 1;">
                            <h3><i class="fas fa-bell" style="margin-right: 10px;"></i> Notifications</h3>
                            <p>Manage alerts and updates</p>
                        </div>
                        <i class="fas fa-chevron-right"></i>
                    </div>
                    
                    <div class="taxi-card">
                        <div style="flex: 1;">
                            <h3><i class="fas fa-shield-alt" style="margin-right: 10px;"></i> Safety Settings</h3>
                            <p>Configure emergency contacts</p>
                        </div>
                        <i class="fas fa-chevron-right"></i>
                    </div>
                    
                    <!-- Load Shedding Settings -->
                    <div class="taxi-card">
                        <div style="flex: 1;">
                            <h3><i class="fas fa-bolt" style="margin-right: 10px;"></i> Load Shedding Alerts</h3>
                            <p>Configure load shedding notifications</p>
                        </div>
                        <i class="fas fa-chevron-right"></i>
                    </div>
                    
                    <div class="ad-container">
                        <div class="ad-label">Sponsored</div>
                        <div class="ad-content">
                            <div class="ad-icon" style="background: #f59e0b;">
                                <i class="fas fa-bolt"></i>
                            </div>
                            <div class="ad-text">
                                <h4>Load Shedding Solutions</h4>
                                <p>Get 20% off solar power banks with code TAXIGUIDE</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="bottom-nav">
                <div class="nav-item active" data-target="mapScreen">
                    <i class="fas fa-map"></i>
                    <span>Map</span>
                </div>
                <div class="nav-item" data-target="associationsScreen">
                    <i class="fas fa-building"></i>
                    <span>Associations</span>
                </div>
                <div class="nav-item" data-target="routesScreen">
                    <i class="fas fa-route"></i>
                    <span>Routes</span>
                </div>
                <div class="nav-item driver-nav" data-target="driverDashboard" id="driverNavItem">
                    <i class="fas fa-taxi"></i>
                    <span>Driver</span>
                </div>
                <div class="nav-item association-nav" data-target="associationDashboard" id="associationNavItem">
                    <i class="fas fa-users"></i>
                    <span>Association</span>
                </div>
            </div>
        </div>
    </div>

    <script>
        // App functionality
        document.addEventListener('DOMContentLoaded', function() {
            // DOM elements
            const authScreen = document.getElementById('authScreen');
            const mainApp = document.getElementById('mainApp');
            const loginBtn = document.getElementById('loginBtn');
            const menuBtn = document.getElementById('menuBtn');
            const closeMenuBtn = document.getElementById('closeMenuBtn');
            const menu = document.getElementById('menu');
            const menuBackdrop = document.getElementById('menuBackdrop');
            const navItems = document.querySelectorAll('.nav-item');
            const menuItems = document.querySelectorAll('.menu-item');
            const screens = document.querySelectorAll('.screen');
            const taxiList = document.getElementById('taxiList');
            const associationList = document.getElementById('associationList');
            const map = document.getElementById('map');
            const locateBtn = document.getElementById('locateBtn');
            const offlineIndicator = document.getElementById('offlineIndicator');
            const emergencyBtn = document.getElementById('emergencyBtn');
            const dataSaverToggle = document.getElementById('dataSaverToggle');
            const upgradeBtn = document.getElementById('upgradeBtn');
            const premiumOffer = document.getElementById('premiumOffer');
            const userTypeBadge = document.getElementById('userTypeBadge');
            const userTypeBtns = document.querySelectorAll('.user-type-btn');
            const driverDashboardMenuItem = document.getElementById('driverDashboardMenuItem');
            const associationDashboardMenuItem = document.getElementById('associationDashboardMenuItem');
            const driverNavItem = document.getElementById('driverNavItem');
            const associationNavItem = document.getElementById('associationNavItem');
            const menuUserName = document.getElementById('menuUserName');
            const menuUserEmail = document.getElementById('menuUserEmail');
            const userAvatar = document.getElementById('userAvatar');
            const alertsContainer = document.getElementById('alertsContainer');
            const clearAlertsBtn = document.getElementById('clearAlertsBtn');
            const noAlertsMessage = document.getElementById('noAlertsMessage');
            
            // App state
            let isPremium = false;
            let isOnline = true;
            let dataSaverEnabled = false;
            let userPosition = { lat: -26.2041, lng: 28.0473 }; // Johannesburg
            let userType = 'passenger'; // passenger, driver, association
            let activeAlerts = [];
            
            // Toggle menu function
            function toggleMenu() {
                menu.classList.toggle('active');
                menuBackdrop.classList.toggle('active');
                document.body.classList.toggle('menu-open');
            }
            
            // Close menu function
            function closeMenu() {
                menu.classList.remove('active');
                menuBackdrop.classList.remove('active');
                document.body.classList.remove('menu-open');
            }
            
            // Set user type
            userTypeBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    userTypeBtns.forEach(b => b.classList.remove('active'));
                    this.classList.add('active');
                    userType = this.getAttribute('data-type');
                });
            });
            
            // Login functionality
            loginBtn.addEventListener('click', function() {
                authScreen.classList.remove('active');
                mainApp.classList.add('active');
                
                // Set user type UI
                userTypeBadge.textContent = userType.charAt(0).toUpperCase() + userType.slice(1);
                if (userType === 'driver') {
                    userTypeBadge.classList.add('user-type-badge');
                    userTypeBadge.classList.remove('association-badge');
                    userAvatar.classList.add('driver-avatar');
                } else if (userType === 'association') {
                    userTypeBadge.classList.add('association-badge');
                    userAvatar.classList.add('association-avatar');
                } else {
                    userTypeBadge.classList.remove('user-type-badge', 'association-badge');
                    userAvatar.classList.remove('driver-avatar', 'association-avatar');
                }
                
                // Set menu items based on user type
                if (userType === 'driver') {
                    menuUserName.textContent = "Thabo Mbeki";
                    menuUserEmail.textContent = "thabo.mbeki@taxiguide.co.za";
                    driverDashboardMenuItem.style.display = 'flex';
                    driverNavItem.style.display = 'flex';
                    associationDashboardMenuItem.style.display = 'none';
                    associationNavItem.style.display = 'none';
                } else if (userType === 'association') {
                    menuUserName.textContent = "Metro Taxis Association";
                    menuUserEmail.textContent = "admin@metrotaxis.co.za";
                    associationDashboardMenuItem.style.display = 'flex';
                    associationNavItem.style.display = 'flex';
                    driverDashboardMenuItem.style.display = 'none';
                    driverNavItem.style.display = 'none';
                } else {
                    menuUserName.textContent = "Martin Memela";
                    menuUserEmail.textContent = "memelasfisomartin@gmail.com";
                    driverDashboardMenuItem.style.display = 'none';
                    driverNavItem.style.display = 'none';
                    associationDashboardMenuItem.style.display = 'none';
                    associationNavItem.style.display = 'none';
                }
                
                initMap();
                generateTaxis();
                generateAssociations();
                startRealTimeUpdates();
                generateAlerts();
            });
            
            // Menu toggle
            menuBtn.addEventListener('click', toggleMenu);
            closeMenuBtn.addEventListener('click', closeMenu);
            
            // Close menu when clicking on backdrop
            menuBackdrop.addEventListener('click', closeMenu);
            
            // Navigation
            navItems.forEach(item => {
                item.addEventListener('click', function() {
                    const target = this.getAttribute('data-target');
                    
                    // Update active nav item
                    navItems.forEach(nav => nav.classList.remove('active'));
                    this.classList.add('active');
                    
                    // Show target screen
                    screens.forEach(screen => {
                        if (screen.id !== 'mainApp' && screen.id !== 'authScreen') {
                            screen.classList.remove('active');
                        }
                    });
                    document.getElementById(target).classList.add('active');
                    
                    closeMenu();
                });
            });
            
            // Menu navigation
            menuItems.forEach(item => {
                if (item.getAttribute('data-target')) {
                    item.addEventListener('click', function(e) {
                        e.preventDefault();
                        const target = this.getAttribute('data-target');
                        
                        // Show target screen
                        screens.forEach(screen => {
                            if (screen.id !== 'mainApp' && screen.id !== 'authScreen') {
                                screen.classList.remove('active');
                            }
                        });
                        document.getElementById(target).classList.add('active');
                        
                        // Update active nav item
                        navItems.forEach(nav => {
                            nav.classList.remove('active');
                            if (nav.getAttribute('data-target') === target) {
                                nav.classList.add('active');
                            }
                        });
                        
                        closeMenu();
                    });
                }
            });
            
            // Initialize the map
            function initMap() {
                // Create map container
                map.innerHTML = '';
                map.innerHTML = '<div class="map-overlay"><div>Johannesburg</div><div class="map-controls"><button class="map-btn" id="locateBtn"><i class="fas fa-location-arrow"></i></button><button class="map-btn"><i class="fas fa-search"></i></button></div></div>';
                
                // Add user marker
                const userMarker = document.createElement('div');
                userMarker.className = 'user-marker';
                userMarker.style.left = '50%';
                userMarker.style.top = '50%';
                map.appendChild(userMarker);
                
                // Add taxi markers
                addTaxiMarkers();
            }
            
            // Add taxi markers to map
            function addTaxiMarkers() {
                for (let i = 0; i < 15; i++) {
                    const isLocal = Math.random() > 0.3;
                    const isVerified = Math.random() > 0.7;
                    const marker = document.createElement('div');
                    marker.className = isLocal ? 'taxi-marker local-taxi' : 'taxi-marker long-distance-taxi';
                    if (isVerified) marker.classList.add('verified-taxi');
                    marker.style.left = `${10 + Math.random() * 80}%`;
                    marker.style.top = `${10 + Math.random() * 80}%`;
                    marker.innerHTML = isLocal ? 'L' : 'LD';
                    map.appendChild(marker);
                }
            }
            
            // Generate taxi list
            function generateTaxis() {
                taxiList.innerHTML = '';
                
                const taxiData = [
                    { 
                        registration: 'ABC 123 GP', 
                        association: 'Metro Taxis', 
                        route: 'City Center - North Suburbs', 
                        distance: '1.2km', 
                        eta: '3 min', 
                        isLocal: true, 
                        isVerified: true,
                        loadshedding: 'Stage 4 (06:00-08:30, 16:00-18:30)'
                    },
                    { 
                        registration: 'XYZ 789 GP', 
                        association: 'Express Shuttles', 
                        route: 'Johannesburg - Pretoria', 
                        distance: '2.5km', 
                        eta: '5 min', 
                        isLocal: false, 
                        isVerified: false,
                        loadshedding: 'Stage 2 (10:00-12:30)'
                    },
                    { 
                        registration: 'DEF 456 GP', 
                        association: 'Urban Transport', 
                        route: 'East Side - West End', 
                        distance: '0.8km', 
                        eta: '2 min', 
                        isLocal: true, 
                        isVerified: true,
                        loadshedding: 'Stage 3 (14:00-16:30)'
                    },
                    { 
                        registration: 'GHI 101 GP', 
                        association: 'Long Haul Commuters', 
                        route: 'Johannesburg - Durban', 
                        distance: '3.1km', 
                        eta: '7 min', 
                        isLocal: false, 
                        isVerified: false,
                        loadshedding: 'Stage 4 (18:00-20:30)'
                    }
                ];
                
                taxiData.forEach((taxi, index) => {
                    // Show only verified taxis for premium users
                    if (isPremium && !taxi.isVerified) return;
                    
                    const card = document.createElement('div');
                    card.className = taxi.isLocal ? 'taxi-card local' : 'taxi-card long-distance';
                    card.innerHTML = `
                        ${taxi.isVerified ? '<div class="premium-badge">Verified</div>' : ''}
                        <div class="taxi-icon">
                            <i class="fas fa-bus"></i>
                        </div>
                        <div class="taxi-info">
                            <h3>${taxi.registration} <span class="taxi-type">${taxi.isLocal ? 'Local' : 'Long Distance'}</span></h3>
                            <p>${taxi.association} • ${taxi.route}</p>
                            <div class="taxi-meta">
                                <span><i class="fas fa-road"></i> ${taxi.distance}</span>
                                <span><i class="fas fa-clock"></i> ETA: ${taxi.eta}</span>
                            </div>
                            <div class="loadshedding-info">
                                <i class="fas fa-bolt"></i>
                                <span>Load Shedding: ${taxi.loadshedding}</span>
                            </div>
                        </div>
                        <div class="taxi-eta">${taxi.eta}</div>
                    `;
                    taxiList.appendChild(card);
                    
                    // Insert ad after every 2 taxis
                    if (index % 2 === 1 && !isPremium) {
                        const ad = document.createElement('div');
                        ad.className = 'ad-container';
                        ad.innerHTML = `
                            <div class="ad-label">Advertisement</div>
                            <div class="ad-content">
                                <div class="ad-icon">
                                    <i class="fas fa-mobile-alt"></i>
                                </div>
                                <div class="ad-text">
                                    <h4>Get Premium for R49/month</h4>
                                    <p>Remove ads and get verified taxis</p>
                                </div>
                            </div>
                        `;
                        taxiList.appendChild(ad);
                    }
                });
            }
            
            // Generate associations list
            function generateAssociations() {
                associationList.innerHTML = '';
                
                const associationData = [
                    { name: 'Metro Taxis', region: 'Johannesburg CBD', taxis: 250, routes: 12 },
                    { name: 'Express Shuttles', region: 'Gauteng Province', taxis: 180, routes: 8 },
                    { name: 'Urban Transport', region: 'Soweto', taxis: 320, routes: 15 },
                    { name: 'Long Haul Commuters', region: 'National', taxis: 150, routes: 5 },
                    { name: 'City Commuters', region: 'Pretoria Central', taxis: 210, routes: 10 },
                    { name: 'Rapid Transit', region: 'Cape Town Metro', taxis: 190, routes: 9 }
                ];
                
                associationData.forEach(assoc => {
                    const card = document.createElement('div');
                    card.className = 'association-card';
                    card.innerHTML = `
                        <div class="association-logo">${assoc.name.charAt(0)}</div>
                        <div class="association-info">
                            <h3>${assoc.name}</h3>
                            <p>${assoc.region}</p>
                            <div class="association-stats">
                                <span><i class="fas fa-bus"></i> ${assoc.taxis} taxis</span>
                                <span><i class="fas fa-route"></i> ${assoc.routes} routes</span>
                            </div>
                        </div>
                        <i class="fas fa-chevron-right"></i>
                    `;
                    associationList.appendChild(card);
                });
            }
            
            // Real-time updates
            let updateInterval;
            function startRealTimeUpdates() {
                updateInterval = setInterval(() => {
                    if (!isOnline) return;
                    
                    // Simulate taxi movement
                    const markers = document.querySelectorAll('.taxi-marker');
                    markers.forEach(marker => {
                        // Random small movement
                        const left = parseFloat(marker.style.left);
                        const top = parseFloat(marker.style.top);
                        
                        marker.style.left = `${left + (Math.random() - 0.5)}%`;
                        marker.style.top = `${top + (Math.random() - 0.5)}%`;
                    });
                    
                    // Simulate changing ETAs
                    const etaElements = document.querySelectorAll('.taxi-eta');
                    etaElements.forEach(eta => {
                        const current = parseInt(eta.textContent);
                        const change = Math.floor(Math.random() * 3) - 1; // -1, 0, or 1
                        const newEta = Math.max(1, current + change);
                        eta.textContent = `${newEta} min`;
                    });
                    
                    // Occasionally add/remove alerts
                    if (Math.random() > 0.7 && activeAlerts.length < 3) {
                        addRandomAlert();
                    } else if (Math.random() > 0.9 && activeAlerts.length > 0) {
                        removeAlert(activeAlerts[0].id);
                    }
                }, 5000); // Update every 5 seconds
            }
            
            // Locate button
            locateBtn.addEventListener('click', function() {
                alert('Locating your position...');
                // In a real app, this would trigger geolocation API
            });
            
            // Emergency button
            emergencyBtn.addEventListener('click', function() {
                const confirm = window.confirm('Send emergency alert with your location to authorities?');
                if (confirm) {
                    alert('Emergency alert sent! Authorities notified of your location.');
                }
            });
            
            // Data saver toggle
            dataSaverToggle.addEventListener('change', function() {
                dataSaverEnabled = this.checked;
                if (dataSaverEnabled) {
                    alert('Data saver mode enabled. Map details reduced.');
                }
            });
            
            // Network status detection
            function updateNetworkStatus() {
                isOnline = navigator.onLine;
                if (isOnline) {
                    offlineIndicator.style.display = 'none';
                } else {
                    offlineIndicator.style.display = 'block';
                    alert('You are offline. Using cached data.');
                }
            }
            
            window.addEventListener('online', updateNetworkStatus);
            window.addEventListener('offline', updateNetworkStatus);
            updateNetworkStatus();
            
            // Initialize the app
            initMap();
            
            // NEW ALERT FUNCTIONALITY
            function generateAlerts() {
                // Clear existing alerts
                alertsContainer.innerHTML = '';
                activeAlerts = [];
                
                // Generate sample alerts
                const alertTypes = ['strike', 'roadworks', 'protest', 'loadshedding'];
                const alertCount = Math.floor(Math.random() * 3); // 0-2 alerts
                
                if (alertCount === 0) {
                    noAlertsMessage.style.display = 'block';
                    return;
                }
                
                noAlertsMessage.style.display = 'none';
                
                for (let i = 0; i < alertCount; i++) {
                    const type = alertTypes[Math.floor(Math.random() * alertTypes.length)];
                    addAlert(type);
                }
            }
            
            function addAlert(type) {
                const id = 'alert-' + Date.now();
                let alertData;
                
                switch(type) {
                    case 'strike':
                        alertData = {
                            id: id,
                            type: 'strike',
                            icon: 'fas fa-ban',
                            title: 'Taxi Strike',
                            description: 'Taxi strike reported in Johannesburg CBD. Expect delays and route changes.',
                            location: 'Johannesburg CBD',
                            severity: 'High'
                        };
                        break;
                    case 'roadworks':
                        alertData = {
                            id: id,
                            type: 'roadworks',
                            icon: 'fas fa-cone',
                            title: 'Road Works',
                            description: 'Major road construction on M1 highway. Reduce speed and expect delays.',
                            location: 'M1 Highway',
                            severity: 'Medium'
                        };
                        break;
                    case 'protest':
                        alertData = {
                            id: id,
                            type: 'protest',
                            icon: 'fas fa-users',
                            title: 'Protest Activity',
                            description: 'Public protest reported in Soweto area. Roads blocked, avoid the area.',
                            location: 'Soweto',
                            severity: 'High'
                        };
                        break;
                    case 'loadshedding':
                        alertData = {
                            id: id,
                            type: 'loadshedding',
                            icon: 'fas fa-bolt',
                            title: 'Load Shedding',
                            description: 'Stage 4 load shedding in effect. Traffic lights may be out of order.',
                            location: 'Johannesburg Metro',
                            severity: 'Medium'
                        };
                        break;
                }
                
                activeAlerts.push(alertData);
                renderAlert(alertData);
                addAlertToMap(alertData);
            }
            
            function renderAlert(alertData) {
                noAlertsMessage.style.display = 'none';
                
                const alertItem = document.createElement('div');
                alertItem.className = `alert-item ${alertData.type}-alert`;
                alertItem.id = alertData.id;
                
                alertItem.innerHTML = `
                    <i class="${alertData.icon}"></i>
                    <div class="alert-content">
                        <div class="alert-title">${alertData.title} - ${alertData.location}</div>
                        <div class="alert-description">${alertData.description}</div>
                    </div>
                    <button class="close-alert" data-id="${alertData.id}">
                        <i class="fas fa-times"></i>
                    </button>
                `;
                
                alertsContainer.appendChild(alertItem);
                
                // Add close functionality

const closeBtn = alertItem.querySelector('.close-alert');
                closeBtn.addEventListener('click', function() {
                    removeAlert(alertData.id);
                });
            }
            
            function removeAlert(id) {
                // Remove from DOM
                const alertElement = document.getElementById(id);
                if (alertElement) {
                    alertElement.remove();
                }
                
                // Remove from active alerts array
                activeAlerts = activeAlerts.filter(alert => alert.id !== id);
                
                // Remove from map
                const mapAlert = document.querySelector(`.map-alert[data-id="${id}"]`);
                if (mapAlert) {
                    mapAlert.remove();
                }
                
                // Show no alerts message if needed
                if (activeAlerts.length === 0) {
                    noAlertsMessage.style.display = 'block';
                }
            }
            
            function addRandomAlert() {
                const alertTypes = ['strike', 'roadworks', 'protest', 'loadshedding'];
                const type = alertTypes[Math.floor(Math.random() * alertTypes.length)];
                addAlert(type);
            }
            
            function addAlertToMap(alertData) {
                const mapAlert = document.createElement('div');
                mapAlert.className = `map-alert ${alertData.type}-map-alert`;
                mapAlert.setAttribute('data-id', alertData.id);
                mapAlert.style.left = `${20 + Math.random() * 60}%`;
                mapAlert.style.top = `${20 + Math.random() * 60}%`;
                mapAlert.innerHTML = `<i class="${alertData.icon}"></i>`;
                mapAlert.title = `${alertData.title}: ${alertData.description}`;
                
                map.appendChild(mapAlert);
            }
            
            // Clear all alerts
            clearAlertsBtn.addEventListener('click', function() {
                activeAlerts.forEach(alert => {
                    removeAlert(alert.id);
                });
            });
            
            // Initialize with some alerts
            setTimeout(() => {
                if (mainApp.classList.contains('active')) {
                    addAlert('strike');
                    addAlert('loadshedding');
                }
            }, 2000);
        });
    </script>
</body>
</html># Aphiwee2012