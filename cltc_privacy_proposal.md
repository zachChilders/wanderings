# A System for Anonymous Data Submission and Robust Access Control

## State of Industry

Modern data collection occurs for a wide variety of innacuass reasons.  Typical uses are diagnostic for purposes of application performance, or experimental for determining
how well users perceive new UX updates.  No matter the use case, the data collected are often personally identifying.  These data are also invaluable to business operations
of software producers, and while larger organizations (Google, Microsoft, Amazon) have proper privacy infrastructure in place, awareness that there are best practices in anonymous
data collection is low amongst smaller organizations.  It is also common that those that are aware of best practices cannot or do not follow them due to time constraints, added cost,
and lack of expertise.  Fewer still restrict access to the collected data correctly, using rudimentary access control if any to see the data.  This leaves collected data vulnerable to
attackers who may be able to access the data by bypassing the access control and attacking the database directly.  Such access controls are often tied not to data access itself, but 
:existing organizational controls and therefore reflect reporting structures rather than access needs.  This model is convenient, but does not lend itself to short term, "need to know"
access and often is implemented as a discrete layer on top of the data storage that may be bypassed rather than an integrated system.

## Proposal

This paper describes a system that both reasonably anonymizes data collected and cryptographically controls access to data collected.  This system combines state of the art techniques
to produce an accessible solution that promotes both security and privacy without compromising usability.  

- Data access via Shamir Secret Sharing
- Differential Privacy via Input Pertubation
- Differential Privacy via Output Pertubation

Additionally, this system emphasizes free (as in beer and speech) distribution and low friction integration models in order to promote widespread adoption.  While many novel techniques
exist both in industry and academia, novelty is not enough to convince organizations to adopt a new technique, especially if that technique imposes overhead without realizing tangible
economic advantages for the adopter.  As such, focus must be placed around cost in terms of both adopting the system initially and maintaining it longterm.  This cost must be
optimized to reduce user facing complexity, performance overhead, and financial costs associated with hosting the system.

## Data Access

Current best case implementations of access controls to data tend to be RBAC based, sometimes with a JIT component.  This approach often includes an Active Directory or LDAP implementation
to ensure that a user is included in the correct access group to be viewing and using the data.  For practical reasons, these access groups tend to follow management hierarchies within an
organization, reflecting reporting chains to ensure that teams that need access to resources will have them.  These structures are often imperfect, and some teams will opt to implement a Just In Time
solution.  The JIT approach requires additional overhead on top of the LDAP or AD services.  It involves a user submitting a request to a service which will add them to a special, time-limited security
group with additional access after a colleauge or manager approves the request.  After the time limit elapses, the user is automatically removed from this group.

While thes approaches ensure that access to data is controlled and audited, it is usually implemented as a layer on top of an already existing datastore.  This inherent decoupling means that should 
the datastore ever be compromised, the protections the access control provide to the data can be avoided by simply stealing the DB layer underneath the ACL.  Additionally, it introduces
risk on the users whose accounts now can be compromised to access the data.  In order to mitigate these issues, we introduce a layer of secure multiparty encryption to the datastore.  

By implementing Shamir's Secret Sharing [Cite], we can keep the familiar workflow of a JIT elevation request but are able to integrate the access grant process into the encryption mechanism
that protects the data to begin with.  This means that the data cannot be decrypted without both an audit trail and peer agreement that the data can be decrypted and utilized.  This can be implemented
when initializing the database - rows of data can be encrypted using a secret shared between all admins.  Depending on the classification of the data, any number of other approvals can be required
to access the data.  This methodology can still be implemented as a seperate layer, but it forces all data access to go through approved channels.


## Input Pertubation

Input Pertubation is an implementation of Differential Privacy that modifies data input into a datastore in order to protect the privacy of the source the data originated from.  There
are multiple methods to this, but one of the simplest to implement is randomized response.  Rather than directly store metrics or results in the datastore, randomized response abstracts
the metric away and will only store whether or not the datapoint belongs to a randomly chosen group.  This layer of abstraction allows for utility in the stored data over a large sample size
as the population metrics will distribute similarly to a direct sample population.  However, the abstraction does not store as many identifying characteristics of the datapoint because
it is simply testing for presence of a given group.

Rather than collect data directly from users, an optional module can be configured to better anonymize data before it gets put into the datastore.  This module is destructive to 
the data, but provides strong privacy guarantees to the users that produce the data. Over a large sample size the destructive property is amortized to be an accurate statistical representation
of the underlying data, but for smaller data sets it can be detrimental to analytics produced on the data.  Due to this property, input pertubation is strictly optional.


## Output Pertubation

Output pertubation is another implementation of Differential Privacy that modifies data read from a datastore.  Instead of allowing direct access to the data in question, analysts are able
to query the data for aggregate information.  By adding minor statistical noise to this data, individual samples in the dataset can be given plausible deniability of actually being in the results.
However, this noise is consistent accross the dataset, which allows for very little utility loss to the analysts, especially on larger datasets.  Laplacian noise is a natural fit for this
purpose.  Because Laplacian distributions can be calculated based off of the size of the datastore in question, the amount of noise added automatically scales to meet the size of the 
dataset.  Due to this property, output pertubation is default.


## Ease of Access

In order to promote adoption of such a system, care must be taken to ensure ease of implemenation within existing organizations and architectures.  To achieve this, the system will
be implemented as a standalone proxy service with connectors to several common datastores.  An existing client should be able to simply change the connection string of the database that it
communicates with and have all queries routed through the proxy service.  The service will then enforce access, add noise to data reads, and optionally randomize writes.  Algorithmically,
these processes can be achieved in a manner that will not have notable impact on performance of the datastore.  By staying consistent in a drop in archicture with little performance and
financial overhead, an open source version of this system can be widely implemented to spread good data practices amongst organizations that may not have the expertise to implement it themselves.
