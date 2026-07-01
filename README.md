<!-- This is the markdown template for the final project of the Building AI course, 
created by Reaktor Innovations and University of Helsinki. 
Copy the template, paste it to your GitHub README and edit! -->

# AnyMCP — Multi-Server MCP Agent for IT Infrastructure Management

Final project for the Building AI course

## Summary

AnyMCP is an AI-powered framework that enables IT administrators to manage and interact 
with their infrastructure through a conversational LLM agent backed by multiple MCP 
(Model Context Protocol) servers. By unifying diverse IT tools and services under a 
single agent interface, it simplifies complex infrastructure operations into natural 
language interactions.


## Background

Modern IT infrastructure is fragmented across multiple platforms, tools, and APIs — 
each requiring its own expertise, CLI syntax, or dashboard. This creates operational 
overhead, slows down troubleshooting, and increases the risk of human error.

Key problems this project addresses:

* **Complexity overload** — Administrators must context-switch between dozens of tools 
  (network devices, cloud platforms, monitoring systems, ticketing systems) to perform 
  routine tasks.
* **Slow incident response** — Correlating data from multiple sources manually takes 
  time, delaying resolution of critical infrastructure issues.
* **Skill gap & knowledge silos** — Not every team member has deep expertise across all 
  infrastructure domains; an LLM agent can bridge that gap.
* **Lack of unified automation interface** — Existing automation tools (Ansible, 
  Terraform, etc.) require structured input and scripting; they are not accessible via 
  natural language.

My personal motivation comes from hands-on experience managing complex network 
environments, where the friction of jumping between tools — routers, switches, 
monitoring dashboards, and ticketing systems — costs valuable time during incidents. 
AnyMCP aims to put a single intelligent interface in front of all of it.


## How is it used?

The administrator interacts with an LLM agent through a chat interface (CLI or web UI). 
The agent interprets the intent behind each request and routes it to the appropriate 
MCP server, which in turn executes the action against the target infrastructure system.

**Typical use scenarios:**

- A network engineer asks: *"Show me all BGP neighbors that are down across all routers"* 
  — the agent queries the network MCP server and returns a consolidated view.
- An IT admin asks: *"Open a change ticket for the firewall maintenance window tonight"* 
  — the agent calls the ITSM MCP server to create the ticket automatically.
- A DevOps engineer asks: *"What is the CPU utilization trend for the production 
  Kubernetes cluster over the last 24 hours?"* — the agent pulls telemetry from the 
  monitoring MCP server.

**Who are the users?**
- Network Engineers
- System Administrators
- DevOps / SRE Teams
- IT Operations Managers

**Environment:**
The solution is designed for enterprise and service provider IT environments where 
multiple infrastructure domains need to be managed concurrently.

![IT Infrastructure Agent](https://upload.wikimedia.org/wikipedia/commons/5/5e/Sleeping_cat_on_her_back.jpg)
> *(Replace the image above with your own architecture diagram once uploaded to the repo)*

**High-level interaction flow:**

```python
def handle_admin_request(user_input: str):
    """
    Example pseudocode showing the agent routing logic.
    """
    intent = llm_agent.parse_intent(user_input)
    
    mcp_servers = {
        "network":    NetworkMCPServer(),
        "monitoring": MonitoringMCPServer(),
        "itsm":       ITSMMCPServer(),
        "cloud":      CloudMCPServer(),
    }
    
    target_server = intent.get("target_domain")   # e.g. "network"
    action        = intent.get("action")          # e.g. "get_bgp_neighbors"
    params        = intent.get("parameters")      # e.g. {"state": "down"}
    
    result = mcp_servers[target_server].execute(action, params)
    return llm_agent.format_response(result)
