# Admin tasks and procedures

# Dynamics 365 F&SCM - Administration Procedures

## Daily Administration Tasks

### System Health Checks
- **Frequency**: Daily (8:00 AM)
- **Activities**:
  - Verify system availability and responsiveness
  - Check Azure service health dashboard
  - Review overnight batch job completion status
  - Monitor database performance metrics (DTU usage)
  - Verify backup completion status

### Batch Job Monitoring
- Review failed batch jobs and investigate root causes
- Restart failed critical jobs after resolution
- Monitor long-running jobs for performance issues
- Adjust batch job schedules based on system load

### User Support
- Respond to helpdesk tickets within SLA (2 hours for P1 issues)
- Investigate user-reported errors and system issues
- Reset user passwords and unlock accounts
- Grant temporary elevated permissions when needed

## Weekly Administration Tasks

### Performance Review
- Analyze system performance trends
- Review slow-running queries and optimize
- Check database growth and storage capacity
- Review and clean up temporary tables

### Security Review
- Audit user access logs for anomalies
- Review new user access requests
- Disable accounts for terminated employees
- Verify security role assignments

### Data Management
- Monitor data entity performance
- Review data import/export logs
- Clean up old integration staging data
- Archive processed records

## Monthly Administration Tasks

### System Maintenance
- Apply non-critical updates and patches
- Review and update system documentation
- Conduct capacity planning review
- Test disaster recovery procedures (quarterly - rotate months)

### User Access Review
- Conduct comprehensive user access audit
- Remove inactive user accounts (90+ days)
- Review and recertify security role assignments
- Update access control matrix

### Reporting & Analytics
- Generate system usage reports
- Provide management dashboard updates
- Document system changes and configurations
- Track key performance indicators (KPIs)

## User Management Procedures

### Creating New Users

**Process**:
1. Receive approved access request form
2. Create user account in Azure AD
3. Assign appropriate security roles based on job function
4. Configure default legal entity and language preferences
5. Set up email notifications
6. Provide welcome email with login credentials
7. Schedule onboarding training session

**Standard Role Templates**:
- **Finance User**: AP/AR, general ledger inquiry
- **Warehouse Worker**: Inventory movements, picking/packing
- **Procurement Agent**: Purchase orders, vendor management
- **Production Supervisor**: Production orders, shop floor management
- **System Administrator**: Full system access

### Modifying User Access

**Process**:
1. Receive change request with manager approval
2. Review current role assignments
3. Add/remove security roles as needed
4. Document changes in access log
5. Notify user of access changes
6. Update role assignment matrix

### Disabling User Accounts

**Process**:
1. Receive termination/separation notice from HR
2. Disable user account in Azure AD
3. Remove all Dynamics 365 security role assignments
4. Revoke API keys and service accounts if applicable
5. Archive user's personal configurations
6. Document account deactivation

## Backup & Recovery Procedures

### Backup Verification

**Daily Checks**:
- Verify automated backup completion (Production: 2:00 AM)
- Check backup file integrity
- Validate backup size (compare to previous day)
- Ensure geo-redundant replication status

**Backup Types**:
- **Full Database Backup**: Daily at 2:00 AM
- **Differential Backup**: Every 6 hours
- **Transaction Log Backup**: Every 15 minutes
- **Configuration Backup**: Weekly (Sundays)

### Recovery Procedures

#### Database Point-in-Time Recovery

**Use Case**: Recover from data corruption or accidental deletion

**Steps**:
1. Identify recovery point (timestamp)
2. Create service request in LCS
3. Specify source environment and target restore point
4. Microsoft performs restore (2-4 hours)
5. Validate restored data
6. Communicate restoration completion to users

**RTO**: 4 hours  
**RPO**: 15 minutes

#### Full Environment Recovery

**Use Case**: Complete environment failure or disaster

**Steps**:
1. Declare disaster recovery event
2. Initiate failover to secondary Azure region
3. Update DNS records to point to DR environment
4. Validate system functionality
5. Monitor performance and stability
6. Communicate to all users
7. Begin investigation of primary site failure

**RTO**: 4 hours  
**RPO**: 15 minutes

### Testing Recovery Procedures

**Frequency**: Quarterly

**Process**:
1. Schedule maintenance window (non-production hours)
2. Perform test recovery to sandbox environment
3. Validate data integrity and completeness
4. Test application functionality
5. Document results and lessons learned
6. Update procedures based on findings

## Update & Release Management

### Update Types

#### Quality Updates (Monthly)
- Microsoft-released bug fixes and improvements
- Automatic deployment to sandbox first
- 7-day validation period
- Manual promotion to production

#### Service Updates (Every 3 months)
- New features and functionality
- Requires planning and testing
- 30-day advance notification from Microsoft
- User acceptance testing required

### Update Deployment Process

**Phase 1: Planning (Week 1)**
- Review release notes from Microsoft
- Identify impacted business processes
- Assess customization compatibility
- Schedule deployment window

**Phase 2: Sandbox Deployment (Week 2)**
- Microsoft auto-deploys to sandbox
- IT team performs smoke testing
- Business users conduct UAT
- Document any issues or concerns

**Phase 3: Production Deployment (Week 3-4)**
- Create deployment checklist
- Notify all users of maintenance window
- Backup production environment
- Deploy update during maintenance window (Saturday 10 PM - Sunday 6 AM)
- Perform post-deployment validation
- Monitor system for 48 hours
- Communicate completion to users

**Rollback Plan**:
- Point-in-time database restore available
- Previous version deployment package retained
- Emergency contact list maintained

### Hotfix Deployment

**Critical Issues Only** (Security vulnerabilities, system down)

**Process**:
1. Receive hotfix from Microsoft or vendor
2. Test in development environment (2 hours)
3. Deploy to production with change control approval
4. Monitor for 24 hours
5. Document deployment and results

## Monitoring & Alerting

### Real-Time Monitoring

**Critical Alerts** (Immediate Response):
- System unavailability
- Database connectivity issues
- Authentication service failures
- Integration endpoint failures
- Security breach attempts

**Warning Alerts** (Response within 2 hours):
- High CPU/memory usage (>80%)
- Batch job failures
- Slow query performance
- Storage capacity warnings (>75%)
- Unusual user activity patterns

### Monitoring Tools Configuration

**Azure Monitor Alerts**:
- System availability checks (every 5 minutes)
- Performance metric thresholds
- Resource utilization alerts
- Cost anomaly detection

**Dynamics 365 LCS Monitoring**:
- Environment health dashboard
- Telemetry and diagnostics
- SQL query performance insights
- User session analytics

### Incident Response

**Severity Levels**:
- **P1 (Critical)**: System down, no workaround - Response: 15 minutes
- **P2 (High)**: Major functionality impaired - Response: 2 hours
- **P3 (Medium)**: Minor functionality issue - Response: 8 hours
- **P4 (Low)**: Cosmetic or documentation - Response: 5 business days

**Response Process**:
1. Acknowledge incident and log in ticketing system
2. Assess severity and business impact
3. Engage appropriate technical resources
4. Communicate status to stakeholders
5. Implement fix or workaround
6. Verify resolution
7. Document root cause and prevention measures

## Data Management & Maintenance

### Database Maintenance

**Index Optimization**:
- Rebuild fragmented indexes weekly (>30% fragmentation)
- Update statistics on large tables
- Schedule during low-usage windows

**Data Archival**:
- Archive completed transactions older than 7 years
- Move to separate archive database
- Maintain query access to archived data
- Follow data retention policies (legal/compliance)

### Data Quality Management

**Data Validation**:
- Run data consistency checks weekly
- Identify and resolve duplicate records
- Validate master data integrity
- Monitor data entity error logs

**Data Cleanup**:
- Purge temporary staging tables monthly
- Remove orphaned records
- Clean up old batch job history (>90 days)
- Archive system logs older than 1 year

## Documentation Standards

### Required Documentation

**System Documentation**:
- Configuration change log
- Customization inventory
- Integration specifications
- Security role matrix
- Disaster recovery runbook

**Operational Documentation**:
- Standard operating procedures (SOPs)
- Troubleshooting guides
- Known issues and workarounds
- Contact lists and escalation paths

**Update Frequency**: Quarterly review, immediate updates for changes

---
