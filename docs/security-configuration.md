# Security roles and compliance

 # Dynamics 365 F&SCM - Security Configuration

## Security Framework Overview

### Security Model
Dynamics 365 F&SCM uses a role-based access control (RBAC) model with the following hierarchy:
- **Duties** → Collections of privileges
- **Privileges** → Access to specific functions
- **Permissions** → CRUD operations on data entities
- **Data Security Policies** → Row-level security filters

### Security Principles
- **Least Privilege**: Users receive minimum access required for their role
- **Separation of Duties**: Conflicting duties separated across roles
- **Defense in Depth**: Multiple security layers
- **Audit Trail**: All security changes logged

## Security Roles Matrix

### Administrative Roles

#### System Administrator
**Purpose**: Full system access for IT staff

**Key Duties**:
- System administration
- Security administration
- User management
- Database administration
- Integration management

**Access Level**: All modules, all data, all legal entities

**Assignment Criteria**: IT Application Engineers, System Administrators only

---

#### Security Administrator
**Purpose**: Manage user access and security roles

**Key Duties**:
- User provisioning
- Role assignment
- Security configuration
- Access review and audit

**Access Level**: Security module, user management, audit logs

**Assignment Criteria**: IT Security team members

---

### Finance Roles

#### Accounting Manager
**Purpose**: Oversee financial operations and reporting

**Key Duties**:
- General ledger management
- Financial period close
- Financial reporting
- Chart of accounts maintenance
- Budget management

**Access Level**: Finance module (full), read access to procurement and inventory

**Restrictions**:

---

## Data Security Policies

### Legal Entity Segregation
**Policy**: Users can only access data from their assigned legal entities

**Implementation**:
Policy: Legal Entity Access
Query: UserInfo.DataAreaId == CurrentLegalEntity
Applied to: All transactional tables

**Exceptions**: System Administrators, Finance Controllers (multi-entity access)

---

### Row-Level Security Examples

#### Warehouse Location Security
**Purpose**: Restrict warehouse workers to assigned locations

**Implementation**:
Policy: Warehouse Location Security
Query: InventLocation.WarehouseId IN UserAssignedWarehouses
Applied to: Inventory transactions, work orders

---

#### Customer Territory Security
**Purpose**: Sales reps see only their territory customers

**Implementation**:
Policy: Sales Territory Access
Query: CustTable.SalesTerritoryId == UserSalesTerritoryId
Applied to: Customer master, sales orders, quotations

---

#### Vendor Security
**Purpose**: Purchasing agents access assigned vendor groups

**Implementation**:
Policy: Vendor Group Access
Query: VendTable.VendGroup IN UserAssignedVendorGroups
Applied to: Vendor master, purchase orders

---

## Authentication & Access Control

### Azure Active Directory Integration

**Configuration**:
- Single Sign-On (SSO) enabled
- Multi-Factor Authentication (MFA) required for:
  - System Administrators
  - Finance Managers
  - Remote access users
- Conditional Access Policies:
  - Block legacy authentication
  - Require managed devices for sensitive roles
  - Geo-location restrictions for admin accounts

### Password Policy

**Requirements**:
- Minimum 12 characters
- Complexity: Uppercase, lowercase, numbers, special characters
- Password expiry: 90 days
- Cannot reuse last 10 passwords
- Account lockout: 5 failed attempts, 30-minute lockout

### Session Management

**Settings**:
- Session timeout: 8 hours (standard users), 2 hours (admin users)
- Concurrent session limit: 2 per user
- Idle timeout: 30 minutes
- Automatic logout on browser close

---

## Privileged Access Management

### Administrator Account Controls

**Requirements**:
- Separate admin accounts (admin_username) for privileged tasks
- MFA mandatory for all admin accounts
- Admin activity logged and reviewed monthly
- Just-in-Time (JIT) access for temporary elevated permissions

### Service Accounts

**Management**:
- Unique service accounts per integration
- No interactive login allowed
- Password rotation: Every 90 days
- Stored in Azure Key Vault
- Activity monitored and alerted

**Current Service Accounts**:
- `svc_d365_powerbi` - Power BI integration
- `svc_d365_logistics` - 3PL integration
- `svc_d365_salesforce` - CRM synchronization
- `svc_d365_banking` - Payment processing

---

## Segregation of Duties (SoD)

### Conflict Rules

#### Financial Conflicts

**Rule 1: Invoice Processing and Payment**
- **Conflict**: User cannot both enter vendor invoices AND approve payments
- **Enforcement**: System validation on role assignment
- **Mitigation**: Separate AP Clerk and AP Manager roles

**Rule 2: Customer Setup and Credit Management**
- **Conflict**: User cannot both create customers AND set credit limits
- **Enforcement**: Workflow approval required
- **Mitigation**: Sales team creates customers, Finance approves credit

**Rule 3: General Ledger Entry and Reconciliation**
- **Conflict**: User cannot both post journal entries AND perform bank reconciliation
- **Enforcement**: Different role assignments
- **Mitigation**: Accounting clerk posts, Accounting manager reconciles

#### Procurement Conflicts

**Rule 4: Purchase Requisition and Approval**
- **Conflict**: User cannot approve their own purchase requisitions
- **Enforcement**: Workflow validation
- **Mitigation**: Manager approval required

**Rule 5: Vendor Creation and Purchasing**
- **Conflict**: Purchasing agent cannot create vendors they buy from
- **Enforcement**: Role separation
- **Mitigation**: Procurement manager maintains vendor master

#### Inventory Conflicts

**Rule 6: Inventory Adjustment and Approval**
- **Conflict**: User cannot both adjust inventory AND approve adjustments
- **Enforcement**: Workflow approval
- **Mitigation**: Warehouse worker proposes, Manager approves

---

## Audit & Compliance

### Audit Logging

**Logged Activities**:
- User login/logout events
- Security role changes
- Data access (create, read, update, delete)
- Configuration changes
- Failed access attempts
- Privileged operations

**Retention**: 
- Security logs: 90 days online, 7 years archived
- Audit trail: Per legal requirements (typically 7-10 years)

### Audit Reports

**Monthly Reports**:
- User access review report
- Failed login attempts
- Security role changes
- Privileged account usage
- SoD violation attempts

**Quarterly Reports**:
- Comprehensive access certification
- Inactive user accounts
- Orphaned accounts (no manager)
- Excessive privilege review

---

## Compliance Standards

### ISO 27001 Requirements

**Controls Implemented**:
- A.9.1.1 Access control policy ✓
- A.9.2.1 User registration and de-registration ✓
- A.9.2.2 User access provisioning ✓
- A.9.2.3 Management of privileged access rights ✓
- A.9.2.4 User secret authentication information ✓
- A.9.2.5 Review of user access rights ✓
- A.9.2.6 Removal or adjustment of access rights ✓
- A.9.4.1 Information access restriction ✓

### GDPR Compliance

**Personal Data Protection**:
- Data encryption at rest and in transit
- Right to access: User data export capability
- Right to erasure: Data deletion workflows
- Data minimization: Collection limited to necessary fields
- Consent management: Tracked in system
- Data breach notification: Incident response procedures

**Data Subject Rights**:
- Request portal for data access
- 30-day response SLA
- Data portability format: JSON/XML export
- Deletion workflow with retention verification

---

## Security Incident Response

### Incident Categories

**Category 1: Unauthorized Access Attempt**
- Failed login threshold breached
- Access from unusual location
- Service account compromise suspected

**Response**: Lock account, notify security team, investigate source

---

**Category 2: Privilege Escalation**
- User attempts unauthorized role assignment
- Modification of security configuration
- Bypass of approval workflows

**Response**: Immediate account suspension, audit log review, management notification

---

**Category 3: Data Breach**
- Unauthorized data export
- Suspicious query patterns
- Data sent to external email

**Response**: Isolate user, preserve evidence, legal/compliance notification, incident investigation

---

### Incident Response Process

**Phase 1: Detection & Alert** (0-15 minutes)
- Automated alert triggers
- Security team notified
- Initial assessment

**Phase 2: Containment** (15-60 minutes)
- Disable compromised accounts
- Block suspicious IP addresses
- Isolate affected systems

**Phase 3: Investigation** (1-24 hours)
- Audit log analysis
- Scope determination
- Evidence collection

**Phase 4: Remediation** (1-7 days)
- Implement fixes
- Reset credentials
- Update security controls

**Phase 5: Post-Incident** (7-30 days)
- Root cause analysis
- Update procedures
- User awareness training
- Management reporting

---

## Security Maintenance Schedule

### Daily
- Monitor security alerts
- Review failed login attempts
- Check privileged account usage

### Weekly
- Review new user access requests
- Audit security role changes
- Test backup restoration

### Monthly
- User access certification
- Security patch review and deployment
- Incident response drill

### Quarterly
- Comprehensive access review
- SoD conflict analysis
- Penetration testing
- Security awareness training
- Disaster recovery test

### Annually
- Security policy review and update
- External security audit
- Compliance certification renewal
- Risk assessment

---

**Document Version**: 1.0  
**Last Updated**: October 2025  
**Owner**: IT Security & Application Engineering Team  
**Classification**: Confidential - Internal Use Only