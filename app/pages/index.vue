<template>
  <div class="min-h-screen bg-slate-100 text-slate-900">
    <div class="flex min-h-screen">
      <aside
        class="hidden w-[280px] shrink-0 border-r border-slate-200 bg-slate-950 text-white lg:flex lg:flex-col"
      >
        <div class="border-b border-white/10 px-6 py-6">
          <div class="flex items-center gap-3">
            <div
              class="flex h-12 w-12 items-center justify-center rounded-2xl bg-white text-slate-950 shadow-sm"
            >
              <div class="relative flex h-8 w-8 items-center justify-center">
                <div class="absolute inset-y-0 left-0 w-1/2 rounded-l-md bg-slate-950" />
                <div
                  class="absolute inset-y-0 right-0 w-1/2 rounded-r-md border-2 border-slate-950 bg-white"
                />
                <div
                  class="absolute inset-x-0 top-1/2 h-1.5 -translate-y-1/2 bg-slate-950"
                />
              </div>
            </div>
            <div>
              <p class="text-base font-semibold">Harcorp Facility Management</p>
              <p class="text-xs text-slate-400">Property Operations Platform</p>
            </div>
          </div>
        </div>

        <div class="px-4 py-5">
          <button
            @click="intakeOpen = true"
            class="flex w-full items-center justify-center gap-2 rounded-2xl bg-white px-4 py-3 text-sm font-semibold text-slate-950 shadow-sm transition hover:-translate-y-0.5"
          >
            <Plus class="h-4 w-4" /> New Work Order
          </button>
        </div>

        <nav class="flex-1 px-3 pb-6">
          <div class="space-y-1">
            <button
              v-for="item in navItems"
              :key="item.id"
              @click="activeView = item.id"
              :class="
                cx(
                  'flex w-full items-center gap-3 rounded-2xl px-4 py-3 text-left text-sm transition',
                  activeView === item.id
                    ? 'bg-white text-slate-950'
                    : 'text-slate-300 hover:bg-white/5 hover:text-white'
                )
              "
            >
              <component :is="item.icon" class="h-4 w-4" />
              <span class="font-medium">{{ item.label }}</span>
            </button>
          </div>

          <div class="mt-8 rounded-3xl border border-white/10 bg-white/5 p-4">
            <p class="text-xs font-semibold uppercase tracking-[0.18em] text-slate-400">
              AI agents live
            </p>
            <div class="mt-4 space-y-3 text-sm text-slate-200">
              <div class="flex items-center gap-3">
                <Sparkles class="h-4 w-4" /> Maintenance Agent
              </div>
              <div class="flex items-center gap-3">
                <Shield class="h-4 w-4" /> Billing Agent
              </div>
              <div class="flex items-center gap-3">
                <ArrowRight class="h-4 w-4" /> Scheduling Agent
              </div>
              <div class="flex items-center gap-3">
                <Users class="h-4 w-4" /> Customer Service Agent
              </div>
            </div>
          </div>
        </nav>

        <div class="border-t border-white/10 p-4">
          <div class="rounded-3xl bg-white/5 p-4">
            <p class="text-sm font-semibold">Jeremiah Harris</p>
            <p class="mt-1 text-xs text-slate-400">Owner • Workspace Admin</p>
          </div>
        </div>
      </aside>

      <main class="min-w-0 flex-1">
        <header
          class="sticky top-0 z-20 border-b border-slate-200 bg-white/90 backdrop-blur"
        >
          <div
            class="flex flex-wrap items-center justify-between gap-4 px-4 py-4 lg:px-8"
          >
            <div>
              <div
                class="inline-flex items-center gap-2 rounded-full bg-slate-100 px-3 py-1 text-xs font-semibold text-slate-700"
              >
                <Building2 class="h-3.5 w-3.5" /> Workspace / Harcorp LLC
              </div>
              <h1 class="mt-3 text-2xl font-semibold tracking-tight text-slate-950">
                {{ activeNav.label }}
              </h1>
              <p class="mt-1 text-sm text-slate-500">
                ClickUp-style operating platform for sales, operations, and recruiting.
              </p>
            </div>

            <div class="flex flex-wrap items-center gap-3">
              <div class="relative min-w-[260px]">
                <Search
                  class="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-slate-400"
                />
                <input
                  class="w-full rounded-2xl border border-slate-300 bg-slate-50 py-3 pl-10 pr-4 text-sm outline-none focus:border-slate-500"
                  placeholder="Search records, tasks, contacts..."
                />
              </div>
              <button
                class="rounded-2xl border border-slate-300 bg-white p-3 text-slate-600"
              >
                <Filter class="h-4 w-4" />
              </button>
              <button
                class="rounded-2xl border border-slate-300 bg-white p-3 text-slate-600"
              >
                <Bell class="h-4 w-4" />
              </button>
              <button
                @click="intakeOpen = true"
                class="inline-flex items-center gap-2 rounded-2xl bg-slate-950 px-4 py-3 text-sm font-medium text-white"
              >
                <Plus class="h-4 w-4" /> New Record
              </button>
            </div>
          </div>
        </header>

        <div class="px-4 py-6 lg:px-8">
          <template v-if="activeView === 'dashboard'">
            <div class="space-y-6">
              <div class="grid gap-4 md:grid-cols-2 xl:grid-cols-4">
                <StatCard
                  label="Active Facilities"
                  value="24"
                  :icon="Building2"
                  meta="3 under maintenance"
                />
                <StatCard
                  label="Open Work Orders"
                  value="18"
                  :icon="Wrench"
                  meta="5 urgent, 13 routine"
                />
                <StatCard
                  label="Monthly Revenue"
                  value="$47.2K"
                  :icon="FileText"
                  meta="12% increase from last month"
                />
                <StatCard
                  label="Scheduled Tasks"
                  value="34"
                  :icon="CalendarDays"
                  meta="8 due today"
                />
              </div>

              <div class="grid gap-6 xl:grid-cols-[1.35fr_0.95fr]">
                <div
                  class="rounded-[28px] border border-slate-200 bg-white p-5 shadow-sm"
                >
                  <SectionHeader
                    title="Facility Overview"
                    subtitle="Current status of all managed properties"
                  >
                    <template #action>
                      <button
                        @click="intakeOpen = true"
                        class="rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                      >
                        Add Facility
                      </button>
                    </template>
                  </SectionHeader>
                  <div class="grid gap-4 md:grid-cols-2 xl:grid-cols-3">
                    <LeadCard
                      v-for="lead in leads.slice(0, 3)"
                      :key="lead.id"
                      :lead="lead"
                    />
                  </div>
                </div>

                <AiInboxCard :items="aiQueue" />
              </div>

              <div class="grid gap-6 xl:grid-cols-[1fr_1fr]">
                <WorkOrdersCard :work-orders="workOrders" />
                <MaintenanceHistoryCard :history="maintenanceHistory" />
              </div>
            </div>
          </template>

          <template v-else-if="activeView === 'facilities'">
            <div class="space-y-6">
              <SectionHeader
                title="Facility Management"
                subtitle="Monitor and manage all properties in your portfolio"
              >
                <template #action>
                  <button
                    @click="intakeOpen = true"
                    class="rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                  >
                    Add Facility
                  </button>
                </template>
              </SectionHeader>
              <FacilitiesGrid :facilities="facilities" />
            </div>
          </template>

          <template v-else-if="activeView === 'maintenance'">
            <div class="space-y-6">
              <SectionHeader
                title="Maintenance Management"
                subtitle="Track work orders, preventive maintenance, and service history"
              >
                <template #action>
                  <button
                    @click="intakeOpen = true"
                    class="rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                  >
                    New Work Order
                  </button>
                </template>
              </SectionHeader>
              <MaintenanceBoard :workOrders="workOrders" :maintenanceHistory="maintenanceHistory" />
            </div>
          </template>

          <template v-else-if="activeView === 'billing'">
            <div class="space-y-6">
              <SectionHeader
                title="Billing & Invoices"
                subtitle="Manage tenant billing, invoices, payments, and financial reporting"
              >
                <template #action>
                  <button
                    class="inline-flex items-center gap-2 rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                  >
                    <FileText class="h-4 w-4" /> Generate Invoice
                  </button>
                </template>
              </SectionHeader>
              <BillingDashboard :invoices="invoices" :payments="payments" />
            </div>
          </template>

          <template v-else-if="activeView === 'scheduling'">
            <div class="space-y-6">
              <SectionHeader
                title="Maintenance Scheduling"
                subtitle="Schedule preventive maintenance, inspections, and service appointments"
              >
                <template #action>
                  <button
                    class="inline-flex items-center gap-2 rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                  >
                    <CalendarDays class="h-4 w-4" /> Schedule Task
                  </button>
                </template>
              </SectionHeader>
              <SchedulingCalendar :scheduledTasks="scheduledTasks" />
            </div>
          </template>

          <template v-else-if="activeView === 'portal'">
            <div class="space-y-6">
              <SectionHeader
                title="Customer Portal"
                subtitle="Tenant access, service requests, and communication hub"
              >
                <template #action>
                  <button
                    class="inline-flex items-center gap-2 rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                  >
                    <Users class="h-4 w-4" /> Portal Settings
                  </button>
                </template>
              </SectionHeader>
              <CustomerPortal :tenants="tenants" :portalActivity="portalActivity" />
            </div>
          </template>

          <template v-else-if="activeView === 'ai'">
            <div class="space-y-6">
              <SectionHeader
                title="AI Assistant Hub"
                subtitle="Intelligent agents helping with facility management workflows"
              >
                <template #action>
                  <button
                    class="inline-flex items-center gap-2 rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                  >
                    <Bot class="h-4 w-4" /> Configure Agents
                  </button>
                </template>
              </SectionHeader>
              <div class="grid gap-6 xl:grid-cols-[0.95fr_1.05fr]">
                <AiInboxCard :items="aiQueue" />
                <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
                  <SectionHeader
                    title="Facility Management Agents"
                    subtitle="AI-powered assistance for maintenance, billing, and operations"
                  />
                  <div class="space-y-4">
                    <div
                      v-for="item in facilityAgentPrompts"
                      :key="item.title"
                      class="rounded-2xl border border-slate-200 bg-slate-50 p-4"
                    >
                      <p class="text-sm font-semibold text-slate-900">{{ item.title }}</p>
                      <p class="mt-2 text-sm leading-7 text-slate-600">{{ item.body }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </template>

          <template v-else-if="activeView === 'tasks'">
            <div class="space-y-6">
              <SectionHeader
                title="Task center"
                subtitle="Linked tasks across business development, operations, and recruiting"
              />
              <div class="grid gap-6 xl:grid-cols-[1fr_0.95fr]">
                <TasksCard :tasks="myTasks" />
                <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
                  <SectionHeader
                    title="Task automation examples"
                    subtitle="System-created follow-up logic"
                  />
                  <div class="space-y-3 text-sm text-slate-700">
                    <div
                      v-for="item in taskAutomations"
                      :key="item"
                      class="rounded-2xl border border-slate-200 bg-slate-50 p-4"
                    >
                      {{ item }}
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </template>
        </div>
      </main>
    </div>

    <IntakeModal
      v-if="intakeOpen"
      v-model:path="intakePath"
      v-model:summary="intakeSummary"
      @close="intakeOpen = false"
    />
  </div>
</template>

<script setup>
import { computed, defineComponent, h, ref } from "vue"
import {
  Building2,
  LayoutDashboard,
  Briefcase,
  Users,
  Wrench,
  UserPlus,
  Bot,
  ClipboardList,
  Search,
  Bell,
  Plus,
  Filter,
  ArrowRight,
  CheckCircle2,
  Clock3,
  MapPin,
  Mail,
  FileText,
  Shield,
  Sparkles,
  CalendarDays,
  ChevronRight,
  X,
  Wand2,
} from "lucide-vue-next"

const cx = (...classes) => classes.filter(Boolean).join(" ")

const navItems = [
  { id: "dashboard", label: "Dashboard", icon: LayoutDashboard },
  { id: "facilities", label: "Facilities", icon: Building2 },
  { id: "maintenance", label: "Maintenance", icon: Wrench },
  { id: "billing", label: "Billing", icon: FileText },
  { id: "scheduling", label: "Scheduling", icon: CalendarDays },
  { id: "portal", label: "Customer Portal", icon: Users },
  { id: "ai", label: "AI Assistant", icon: Bot },
]

const salesStages = [
  { key: "new", label: "New Lead" },
  { key: "qualified", label: "AI Qualified" },
  { key: "review", label: "Discovery / Review" },
  { key: "proposal", label: "Proposal" },
  { key: "decision", label: "Awaiting Decision" },
  { key: "won", label: "Won" },
]

const recruitingStages = [
  { key: "captured", label: "Prospect Captured" },
  { key: "qualified", label: "AI Pre-Qualified" },
  { key: "licensing", label: "Licensing Path" },
  { key: "interview", label: "Interview Review" },
  { key: "onboarding", label: "Onboarding" },
  { key: "active", label: "Active Contributor" },
]

const leads = [
  {
    id: "LD-2041",
    name: "Montgomery County Facilities",
    contact: "R. Turner",
    email: "rturner@montgomery.example",
    stage: "proposal",
    type: "Contract Review",
    value: "$285,000",
    priority: "High",
    location: "Rockville, MD",
    ai:
      "Portfolio maintenance opportunity across multiple sites. Strong fit with Harcorp contract management model.",
  },
  {
    id: "LD-2037",
    name: "Eastgate Medical Plaza",
    contact: "A. Collins",
    email: "acollins@eastgate.example",
    stage: "qualified",
    type: "Service + PM",
    value: "$74,000",
    priority: "High",
    location: "Washington, DC",
    ai: "Urgent maintenance needs with likely PM expansion. Follow up in 1 business day.",
  },
  {
    id: "LD-2028",
    name: "Trinity Construction Group",
    contact: "C. Arnold",
    email: "carnold@trinity.example",
    stage: "review",
    type: "Partnership",
    value: "$120,000",
    priority: "Medium",
    location: "Baltimore, MD",
    ai:
      "Partnership fit appears strong. Requires scope alignment and vendor coordination discussion.",
  },
  {
    id: "LD-2014",
    name: "PA National Guard Support",
    contact: "M. Alvarez",
    email: "malvarez@png.example",
    stage: "new",
    type: "Kitchen Equipment",
    value: "$190,000",
    priority: "High",
    location: "Fort Indiantown Gap, PA",
    ai:
      "Government maintenance scope. Review access requirements and response expectations before qualification.",
  },
  {
    id: "LD-1988",
    name: "Glebe Handyman Services",
    contact: "Operations",
    email: "ops@glebe.example",
    stage: "won",
    type: "Commercial Referral",
    value: "$42,000",
    priority: "Medium",
    location: "Alexandria, VA",
    ai:
      "Referral partner converted. Create kickoff tasks and vendor coordination checklist.",
  },
]

const crmRecords = [
  {
    org: "Montgomery County Facilities",
    contact: "Rebecca Turner",
    title: "Procurement Lead",
    status: "Active Opportunity",
    region: "MD",
    owner: "Jeremiah Harris",
    nextStep: "Proposal review Thursday",
  },
  {
    org: "Eastgate Medical Plaza",
    contact: "Angela Collins",
    title: "Facilities Director",
    status: "PM Expansion",
    region: "DC",
    owner: "Project Coordinator",
    nextStep: "Schedule discovery call",
  },
  {
    org: "Trinity Construction Group",
    contact: "Colin Arnold",
    title: "Partner Contact",
    status: "Partner Review",
    region: "MD",
    owner: "Jeremiah Harris",
    nextStep: "Scope alignment meeting",
  },
  {
    org: "Prince George's County Site Ops",
    contact: "Dana Hayes",
    title: "Asset Manager",
    status: "Nurture",
    region: "MD",
    owner: "Business Development",
    nextStep: "Send capability statement",
  },
]

const workOrders = [
  {
    id: "HC23-2893",
    client: "Eastgate Medical Plaza",
    category: "Emergency Repair",
    status: "Ready for Dispatch",
    urgency: "Emergency",
    sla: "Due in 2h",
    asset: "Ice machine - kitchen wing",
  },
  {
    id: "HC23-2896",
    client: "Montgomery County Facilities",
    category: "Preventive Maintenance",
    status: "Awaiting Review",
    urgency: "Standard",
    sla: "Due tomorrow",
    asset: "Boiler inspection",
  },
  {
    id: "HC23-2898",
    client: "PA National Guard Support",
    category: "Kitchen Equipment",
    status: "In Progress",
    urgency: "High",
    sla: "On track",
    asset: "Steam kettle diagnosis",
  },
]

const recruits = [
  {
    id: "RC-118",
    name: "Tasha Greene",
    state: "MD",
    stage: "interview",
    licensing: "Licensed",
    fit: 92,
    availability: "20 hrs/week",
    ai: "Strong communication background, licensed, interview-ready.",
  },
  {
    id: "RC-114",
    name: "Marcus Bell",
    state: "DC",
    stage: "licensing",
    licensing: "Not Licensed",
    fit: 81,
    availability: "Evenings",
    ai: "High intent, needs licensing sequence and recruiter follow-up.",
  },
  {
    id: "RC-109",
    name: "Janelle Price",
    state: "VA",
    stage: "qualified",
    licensing: "Licensed",
    fit: 86,
    availability: "Full-time",
    ai: "Good fit for onboarding path after screening.",
  },
  {
    id: "RC-101",
    name: "Damien Clark",
    state: "PA",
    stage: "captured",
    licensing: "Unknown",
    fit: 58,
    availability: "Unknown",
    ai: "Needs more detail before advancing.",
  },
  {
    id: "RC-095",
    name: "Leslie Moore",
    state: "MD",
    stage: "onboarding",
    licensing: "Licensed",
    fit: 94,
    availability: "30 hrs/week",
    ai: "Accepted. Build training checklist and kickoff tasks.",
  },
]

const aiQueue = [
  {
    title: "New service request classified as emergency repair",
    detail: "Eastgate Medical Plaza intake converted to work-order draft HC23-2893.",
    confidence: "96%",
    action: "Dispatch review",
  },
  {
    title: "Government opportunity flagged for contract verification",
    detail: "PA National Guard support inquiry needs access and compliance check.",
    confidence: "82%",
    action: "Compliance review",
  },
  {
    title: "Recruit candidate routed to licensing path",
    detail: "Marcus Bell appears high intent but not yet licensed.",
    confidence: "88%",
    action: "Recruiter follow-up",
  },
]

const myTasks = [
  { title: "Review Montgomery County proposal scope", due: "Today", priority: "High" },
  {
    title: "Call Eastgate Medical Plaza on PM expansion",
    due: "Today",
    priority: "High",
  },
  { title: "Schedule interview with Tasha Greene", due: "Tomorrow", priority: "Medium" },
  {
    title: "Verify vendor credentials for kitchen equipment work",
    due: "Tomorrow",
    priority: "Medium",
  },
]

const maintenanceHistory = [
  { id: "WO-2024-0456", title: "HVAC System Maintenance", facility: "Downtown Office", completed: "2 days ago", cost: "$2,450", technician: "Mike Johnson" },
  { id: "WO-2024-0455", title: "Plumbing Repair - Floor 3", facility: "Medical Plaza", completed: "3 days ago", cost: "$890", technician: "Sarah Davis" },
  { id: "WO-2024-0454", title: "Electrical Panel Upgrade", facility: "Warehouse A", completed: "1 week ago", cost: "$3,200", technician: "Tom Wilson" },
  { id: "WO-2024-0453", title: "Roof Leak Repair", facility: "Retail Center", completed: "1 week ago", cost: "$1,150", technician: "Lisa Chen" },
]

const facilities = [
  { id: "FAC-001", name: "Downtown Office Complex", address: "123 Main St, Downtown", status: "Active", type: "Office", sqFt: "50,000", tenants: 12, lastInspection: "2 weeks ago" },
  { id: "FAC-002", name: "Medical Plaza", address: "456 Health Ave, Medical District", status: "Active", type: "Medical", sqFt: "75,000", tenants: 8, lastInspection: "1 week ago" },
  { id: "FAC-003", name: "Warehouse A", address: "789 Industrial Blvd", status: "Maintenance", type: "Warehouse", sqFt: "100,000", tenants: 2, lastInspection: "3 days ago" },
  { id: "FAC-004", name: "Retail Center", address: "321 Shopping Mall Dr", status: "Active", type: "Retail", sqFt: "40,000", tenants: 15, lastInspection: "1 month ago" },
]

const invoices = [
  { id: "INV-2024-001", tenant: "TechCorp Inc", facility: "Downtown Office", amount: "$12,500", dueDate: "2024-04-15", status: "Paid", issued: "2024-03-15" },
  { id: "INV-2024-002", tenant: "MediCare Plus", facility: "Medical Plaza", amount: "$8,750", dueDate: "2024-04-20", status: "Pending", issued: "2024-03-20" },
  { id: "INV-2024-003", tenant: "Logistics Pro", facility: "Warehouse A", amount: "$15,200", dueDate: "2024-04-10", status: "Overdue", issued: "2024-03-10" },
  { id: "INV-2024-004", tenant: "Retail Group", facility: "Retail Center", amount: "$6,800", dueDate: "2024-04-25", status: "Paid", issued: "2024-03-25" },
]

const payments = [
  { id: "PAY-2024-001", tenant: "TechCorp Inc", amount: "$12,500", method: "ACH", date: "2024-04-10", invoice: "INV-2024-001" },
  { id: "PAY-2024-002", tenant: "MediCare Plus", amount: "$4,375", method: "Check", date: "2024-04-12", invoice: "INV-2024-002" },
  { id: "PAY-2024-003", tenant: "Retail Group", amount: "$6,800", method: "Wire", date: "2024-04-08", invoice: "INV-2024-004" },
]

const scheduledTasks = [
  { id: "SCH-001", title: "HVAC Filter Replacement", facility: "Downtown Office", date: "2024-04-25", time: "9:00 AM", technician: "Mike Johnson", type: "Preventive" },
  { id: "SCH-002", title: "Fire Alarm Inspection", facility: "Medical Plaza", date: "2024-04-26", time: "2:00 PM", technician: "Sarah Davis", type: "Inspection" },
  { id: "SCH-003", title: "Elevator Maintenance", facility: "Retail Center", date: "2024-04-28", time: "8:00 AM", technician: "Tom Wilson", type: "Preventive" },
  { id: "SCH-004", title: "Plumbing Check", facility: "Warehouse A", date: "2024-04-30", time: "10:00 AM", technician: "Lisa Chen", type: "Inspection" },
]

const tenants = [
  { id: "TEN-001", name: "TechCorp Inc", facility: "Downtown Office", contact: "John Smith", email: "john@techcorp.com", portalAccess: true, lastLogin: "2 days ago" },
  { id: "TEN-002", name: "MediCare Plus", facility: "Medical Plaza", contact: "Dr. Sarah Johnson", email: "sarah@medicare.com", portalAccess: true, lastLogin: "1 day ago" },
  { id: "TEN-003", name: "Logistics Pro", facility: "Warehouse A", contact: "Mike Davis", email: "mike@logistics.com", portalAccess: false, lastLogin: "Never" },
  { id: "TEN-004", name: "Retail Group", facility: "Retail Center", contact: "Lisa Chen", email: "lisa@retail.com", portalAccess: true, lastLogin: "5 hours ago" },
]

const portalActivity = [
  { id: "ACT-001", tenant: "TechCorp Inc", action: "Submitted service request", details: "HVAC issue in Suite 1201", timestamp: "2 hours ago", status: "New" },
  { id: "ACT-002", tenant: "MediCare Plus", action: "Viewed invoice", details: "Invoice INV-2024-002", timestamp: "4 hours ago", status: "Viewed" },
  { id: "ACT-003", tenant: "Retail Group", action: "Updated contact info", details: "Changed phone number", timestamp: "1 day ago", status: "Completed" },
  { id: "ACT-004", tenant: "TechCorp Inc", action: "Downloaded lease", details: "Lease agreement renewal", timestamp: "2 days ago", status: "Downloaded" },
]

const facilityAgentPrompts = [
  {
    title: "Maintenance Agent",
    body: "Analyze maintenance requests, prioritize by urgency and impact, suggest optimal scheduling, and recommend preventive measures based on historical data.",
  },
  {
    title: "Billing Agent",
    body: "Generate accurate invoices, track payment status, send automated reminders, and provide financial reporting for facility operations.",
  },
  {
    title: "Scheduling Agent",
    body: "Optimize maintenance schedules, coordinate technician availability, minimize tenant disruption, and ensure compliance with regulatory requirements.",
  },
  {
    title: "Customer Service Agent",
    body: "Handle tenant inquiries, process service requests, provide status updates, and maintain communication through the customer portal.",
  },
]

const agentPrompts = [
  {
    title: "Intake Agent",
    body:
      "Collect complete structured information for sales, service, proposal, vendor, and recruiting workflows. Ask concise follow-up questions only when required fields are missing.",
  },
  {
    title: "Qualification Agent",
    body:
      "Classify the record by urgency, value, fit, and next-step routing. Recommend one next action only.",
  },
  {
    title: "Workflow Router",
    body:
      "Map the record into the correct CRM pipeline, board, stage, and owner queue. Create related tasks when appropriate.",
  },
  {
    title: "Recruiting Agent",
    body:
      "Assess seriousness, readiness, licensing status, and fit. Route to nurture, licensing path, interview review, onboarding, or decline.",
  },
]

const taskAutomations = [
  "If AI score is high and opportunity type is proposal, create follow-up within 1 business day.",
  "If urgency is emergency, create top-priority dispatch review task immediately.",
  "If recruit is licensed and fit score exceeds 85, create interview scheduling task automatically.",
  "If onboarding begins, generate training checklist and owner assignments.",
]

const prompts = {
  service: {
    title: "AI Service Intake",
    helper:
      "Capture dispatch-ready details for maintenance, repair, or work-order creation.",
    fields: [
      "Client / organization name",
      "Site address",
      "On-site contact and phone",
      "Urgency level",
      "Asset / equipment type",
      "Make, model, and serial",
      "Issue description",
      "Contract status",
      "Access / security requirements",
    ],
    system:
      "You are Harcorp's service intake agent. Collect complete information for a dispatch-ready work request. Ask concise follow-up questions only when required fields are missing. Output structured JSON with request type, urgency, summary, missing fields, and recommended next action.",
  },
  proposal: {
    title: "AI Proposal Intake",
    helper:
      "Qualify new contract and proposal opportunities before they hit business development.",
    fields: [
      "Organization name",
      "Contact name and email",
      "Region / locations",
      "Services in scope",
      "Number of sites",
      "Budget or contract context",
      "Timeline",
      "Buying need / challenge",
    ],
    system:
      "You are Harcorp's proposal qualification agent. Gather complete information about the scope, geography, timeline, and value of the opportunity. Output structured JSON with opportunity type, estimated tier, next action, and required follow-up.",
  },
  recruit: {
    title: "AI Recruitment Intake",
    helper: "Qualify recruits or partners and route them into the right stage.",
    fields: [
      "Full name",
      "Email and phone",
      "State",
      "Current occupation",
      "Licensing status",
      "Relevant experience",
      "Weekly availability",
      "Reason for interest",
      "Target income goal",
    ],
    system:
      "You are Harcorp's recruiting intake agent. Assess fit, seriousness, licensing status, and readiness. Output structured JSON with fit score, recommended stage, follow-up action, and missing information.",
  },
}

const activeView = ref("dashboard")
const intakeOpen = ref(false)
const intakePath = ref("service")
const intakeSummary = ref("")

const activeNav = computed(
  () => navItems.find((item) => item.id === activeView.value) || navItems[0]
)

const SectionHeader = defineComponent({
  name: "SectionHeader",
  props: {
    title: { type: String, required: true },
    subtitle: { type: String, default: "" },
  },
  setup(props, { slots }) {
    return () =>
      h("div", { class: "mb-5 flex items-start justify-between gap-4" }, [
        h("div", {}, [
          h("h2", { class: "text-xl font-semibold text-slate-950" }, props.title),
          props.subtitle
            ? h("p", { class: "mt-1 text-sm text-slate-500" }, props.subtitle)
            : null,
        ]),
        slots.action ? h("div", {}, slots.action()) : null,
      ])
  },
})

const StatCard = defineComponent({
  name: "StatCard",
  props: {
    label: { type: String, required: true },
    value: { type: String, required: true },
    icon: { type: Object, required: true },
    meta: { type: String, default: "" },
  },
  template: `
    <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
      <div class="flex items-center justify-between">
        <p class="text-sm font-medium text-slate-500">{{ label }}</p>
        <div class="rounded-2xl bg-slate-100 p-2">
          <component :is="icon" class="h-4 w-4 text-slate-700" />
        </div>
      </div>
      <p class="mt-4 text-3xl font-semibold tracking-tight text-slate-950">{{ value }}</p>
      <p class="mt-2 text-sm text-slate-500">{{ meta }}</p>
    </div>
  `,
})

const StagePill = defineComponent({
  name: "StagePill",
  template: `<span class="rounded-full bg-slate-100 px-2.5 py-1 text-xs font-medium text-slate-700"><slot /></span>`,
})

const PriorityBadge = defineComponent({
  name: "PriorityBadge",
  props: { value: { type: String, required: true } },
  setup(props) {
    const styles = {
      High: "bg-rose-50 text-rose-700 border-rose-200",
      Medium: "bg-amber-50 text-amber-700 border-amber-200",
      Standard: "bg-slate-100 text-slate-700 border-slate-200",
      Emergency: "bg-rose-50 text-rose-700 border-rose-200",
    }
    const badgeClass = computed(() =>
      cx(
        "rounded-full border px-2.5 py-1 text-xs font-medium",
        styles[props.value] || styles.Standard
      )
    )
    return { badgeClass }
  },
  template: `<span :class="badgeClass">{{ value }}</span>`,
})

const LeadCard = defineComponent({
  name: "LeadCard",
  components: { PriorityBadge, StagePill, MapPin, Mail, ChevronRight },
  props: { lead: { type: Object, required: true } },
  template: `
    <div class="rounded-3xl border border-slate-200 bg-white p-4 shadow-sm transition hover:-translate-y-0.5">
      <div class="flex items-start justify-between gap-3">
        <div>
          <p class="text-sm font-semibold text-slate-950">{{ lead.name }}</p>
          <p class="mt-1 text-xs text-slate-500">{{ lead.id }} • {{ lead.type }}</p>
        </div>
        <PriorityBadge :value="lead.priority" />
      </div>
      <div class="mt-3 space-y-2 text-xs text-slate-600">
        <div class="flex items-center gap-2"><MapPin class="h-3.5 w-3.5" />{{ lead.location }}</div>
        <div class="flex items-center gap-2"><Mail class="h-3.5 w-3.5" />{{ lead.contact }} • {{ lead.email }}</div>
      </div>
      <div class="mt-4 rounded-2xl bg-slate-50 p-3 text-xs leading-6 text-slate-600">
        <span class="font-semibold text-slate-900">AI:</span> {{ lead.ai }}
      </div>
      <div class="mt-4 flex items-center justify-between">
        <StagePill>{{ lead.value }}</StagePill>
        <button class="inline-flex items-center gap-1 text-xs font-medium text-slate-700">
          Open <ChevronRight class="h-3.5 w-3.5" />
        </button>
      </div>
    </div>
  `,
})

const TableCard = defineComponent({
  name: "TableCard",
  components: { StagePill },
  props: { rows: { type: Array, required: true } },
  template: `
    <div class="overflow-hidden rounded-3xl border border-slate-200 bg-white shadow-sm">
      <div class="overflow-x-auto">
        <table class="min-w-full text-left text-sm">
          <thead class="bg-slate-50 text-slate-500">
            <tr>
              <th class="px-5 py-4 font-medium">Organization</th>
              <th class="px-5 py-4 font-medium">Contact</th>
              <th class="px-5 py-4 font-medium">Status</th>
              <th class="px-5 py-4 font-medium">Region</th>
              <th class="px-5 py-4 font-medium">Owner</th>
              <th class="px-5 py-4 font-medium">Next Step</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="row in rows" :key="row.org" class="border-t border-slate-100">
              <td class="px-5 py-4">
                <div>
                  <p class="font-medium text-slate-900">{{ row.org }}</p>
                  <p class="text-xs text-slate-500">Active record</p>
                </div>
              </td>
              <td class="px-5 py-4">
                <div>
                  <p class="text-slate-900">{{ row.contact }}</p>
                  <p class="text-xs text-slate-500">{{ row.title }}</p>
                </div>
              </td>
              <td class="px-5 py-4"><StagePill>{{ row.status }}</StagePill></td>
              <td class="px-5 py-4 text-slate-600">{{ row.region }}</td>
              <td class="px-5 py-4 text-slate-600">{{ row.owner }}</td>
              <td class="px-5 py-4 text-slate-600">{{ row.nextStep }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  `,
})

const WorkOrdersCard = defineComponent({
  name: "WorkOrdersCard",
  components: { SectionHeader, PriorityBadge, StagePill, Clock3 },
  props: { workOrders: { type: Array, required: true } },
  template: `
    <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
      <SectionHeader title="Operations Queue" subtitle="Work orders and service requests ready for review or dispatch" />
      <div class="space-y-3">
        <div v-for="wo in workOrders" :key="wo.id" class="rounded-2xl border border-slate-200 p-4">
          <div class="flex items-start justify-between gap-3">
            <div>
              <p class="text-sm font-semibold text-slate-950">{{ wo.id }} • {{ wo.client }}</p>
              <p class="mt-1 text-xs text-slate-500">{{ wo.category }} • {{ wo.asset }}</p>
            </div>
            <PriorityBadge :value="wo.urgency" />
          </div>
          <div class="mt-3 flex flex-wrap items-center gap-2 text-xs text-slate-600">
            <StagePill>{{ wo.status }}</StagePill>
            <span class="inline-flex items-center gap-1"><Clock3 class="h-3.5 w-3.5" />{{ wo.sla }}</span>
          </div>
        </div>
      </div>
    </div>
  `,
})

const AiInboxCard = defineComponent({
  name: "AiInboxCard",
  components: { SectionHeader, StagePill, Sparkles },
  props: { items: { type: Array, required: true } },
  template: `
    <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
      <SectionHeader title="AI Inbox" subtitle="Recent classification, routing, and exception activity" />
      <div class="space-y-3">
        <div v-for="item in items" :key="item.title" class="rounded-2xl border border-slate-200 bg-slate-50 p-4">
          <div class="flex items-start justify-between gap-3">
            <div>
              <p class="text-sm font-semibold text-slate-900">{{ item.title }}</p>
              <p class="mt-1 text-xs leading-6 text-slate-600">{{ item.detail }}</p>
            </div>
            <StagePill>{{ item.confidence }}</StagePill>
          </div>
          <div class="mt-3 inline-flex items-center gap-2 text-xs font-medium text-slate-700">
            <Sparkles class="h-3.5 w-3.5" /> {{ item.action }}
          </div>
        </div>
      </div>
    </div>
  `,
})

const TasksCard = defineComponent({
  name: "TasksCard",
  components: { SectionHeader, PriorityBadge, CheckCircle2 },
  props: { tasks: { type: Array, required: true } },
  template: `
    <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
      <SectionHeader title="My Tasks" subtitle="Follow-up, recruiting, proposal, and dispatch actions" />
      <div class="space-y-3">
        <div v-for="task in tasks" :key="task.title" class="flex items-center justify-between gap-4 rounded-2xl border border-slate-200 p-4">
          <div class="flex items-start gap-3">
            <CheckCircle2 class="mt-0.5 h-4 w-4 text-slate-400" />
            <div>
              <p class="text-sm font-medium text-slate-900">{{ task.title }}</p>
              <p class="mt-1 text-xs text-slate-500">Due {{ task.due }}</p>
            </div>
          </div>
          <PriorityBadge :value="task.priority" />
        </div>
      </div>
    </div>
  `,
})

const MaintenanceHistoryCard = defineComponent({
  name: "MaintenanceHistoryCard",
  components: { SectionHeader, CheckCircle2, Clock3 },
  props: { history: { type: Array, required: true } },
  template: `
    <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
      <SectionHeader title="Maintenance History" subtitle="Recent completed work and service records" />
      <div class="space-y-3">
        <div v-for="item in history" :key="item.id" class="flex items-center justify-between gap-4 rounded-2xl border border-slate-200 p-4">
          <div class="flex items-start gap-3">
            <CheckCircle2 class="mt-0.5 h-4 w-4 text-green-500" />
            <div>
              <p class="text-sm font-medium text-slate-900">{{ item.title }}</p>
              <p class="mt-1 text-xs text-slate-500">{{ item.facility }} • {{ item.completed }}</p>
            </div>
          </div>
          <div class="text-right">
            <p class="text-sm font-medium text-slate-900">{{ item.cost }}</p>
            <p class="text-xs text-slate-500">{{ item.technician }}</p>
          </div>
        </div>
      </div>
    </div>
  `,
})

const FacilitiesGrid = defineComponent({
  name: "FacilitiesGrid",
  components: { SectionHeader, Building2, MapPin },
  props: { facilities: { type: Array, required: true } },
  template: `
    <div class="grid gap-4 md:grid-cols-2 xl:grid-cols-3">
      <div v-for="facility in facilities" :key="facility.id" class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <div class="flex items-start justify-between gap-3">
          <div class="flex items-start gap-3">
            <Building2 class="mt-1 h-5 w-5 text-slate-400" />
            <div>
              <p class="text-sm font-semibold text-slate-950">{{ facility.name }}</p>
              <p class="mt-1 text-xs text-slate-500 flex items-center gap-1">
                <MapPin class="h-3 w-3" />{{ facility.address }}
              </p>
            </div>
          </div>
          <span :class="facility.status === 'Active' ? 'bg-green-100 text-green-700' : 'bg-yellow-100 text-yellow-700'" class="rounded-full px-2.5 py-1 text-xs font-medium">
            {{ facility.status }}
          </span>
        </div>
        <div class="mt-4 grid grid-cols-2 gap-4 text-sm">
          <div>
            <p class="text-slate-500">Type</p>
            <p class="font-medium text-slate-900">{{ facility.type }}</p>
          </div>
          <div>
            <p class="text-slate-500">Sq Ft</p>
            <p class="font-medium text-slate-900">{{ facility.sqFt }}</p>
          </div>
          <div>
            <p class="text-slate-500">Tenants</p>
            <p class="font-medium text-slate-900">{{ facility.tenants }}</p>
          </div>
          <div>
            <p class="text-slate-500">Last Inspection</p>
            <p class="font-medium text-slate-900">{{ facility.lastInspection }}</p>
          </div>
        </div>
      </div>
    </div>
  `,
})

const MaintenanceBoard = defineComponent({
  name: "MaintenanceBoard",
  components: { SectionHeader, PriorityBadge, StagePill, Clock3, CheckCircle2 },
  props: { workOrders: { type: Array, required: true }, maintenanceHistory: { type: Array, required: true } },
  template: `
    <div class="grid gap-6 xl:grid-cols-[1fr_1fr]">
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <SectionHeader title="Active Work Orders" subtitle="Current maintenance and repair tasks" />
        <div class="space-y-3">
          <div v-for="wo in workOrders" :key="wo.id" class="rounded-2xl border border-slate-200 p-4">
            <div class="flex items-start justify-between gap-3">
              <div>
                <p class="text-sm font-semibold text-slate-950">{{ wo.id }} • {{ wo.client }}</p>
                <p class="mt-1 text-xs text-slate-500">{{ wo.category }} • {{ wo.asset }}</p>
              </div>
              <PriorityBadge :value="wo.urgency" />
            </div>
            <div class="mt-3 flex flex-wrap items-center gap-2 text-xs text-slate-600">
              <StagePill>{{ wo.status }}</StagePill>
              <span class="inline-flex items-center gap-1"><Clock3 class="h-3.5 w-3.5" />{{ wo.sla }}</span>
            </div>
          </div>
        </div>
      </div>
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <SectionHeader title="Recent Completions" subtitle="Completed maintenance work" />
        <div class="space-y-3">
          <div v-for="item in maintenanceHistory.slice(0, 4)" :key="item.id" class="flex items-center justify-between gap-4 rounded-2xl border border-slate-200 p-4">
            <div class="flex items-start gap-3">
              <CheckCircle2 class="mt-0.5 h-4 w-4 text-green-500" />
              <div>
                <p class="text-sm font-medium text-slate-900">{{ item.title }}</p>
                <p class="mt-1 text-xs text-slate-500">{{ item.facility }} • {{ item.completed }}</p>
              </div>
            </div>
            <div class="text-right">
              <p class="text-sm font-medium text-slate-900">{{ item.cost }}</p>
              <p class="text-xs text-slate-500">{{ item.technician }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
})

const BillingDashboard = defineComponent({
  name: "BillingDashboard",
  components: { SectionHeader, FileText, CheckCircle2, Clock3 },
  props: { invoices: { type: Array, required: true }, payments: { type: Array, required: true } },
  template: `
    <div class="grid gap-6 xl:grid-cols-[1fr_1fr]">
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <SectionHeader title="Recent Invoices" subtitle="Outstanding and recent billing" />
        <div class="space-y-3">
          <div v-for="invoice in invoices" :key="invoice.id" class="rounded-2xl border border-slate-200 p-4">
            <div class="flex items-start justify-between gap-3">
              <div>
                <p class="text-sm font-semibold text-slate-950">{{ invoice.id }} • {{ invoice.tenant }}</p>
                <p class="mt-1 text-xs text-slate-500">{{ invoice.facility }} • Issued {{ invoice.issued }}</p>
              </div>
              <span :class="invoice.status === 'Paid' ? 'bg-green-100 text-green-700' : invoice.status === 'Pending' ? 'bg-yellow-100 text-yellow-700' : 'bg-red-100 text-red-700'" class="rounded-full px-2.5 py-1 text-xs font-medium">
                {{ invoice.status }}
              </span>
            </div>
            <div class="mt-3 flex items-center justify-between">
              <div class="text-sm">
                <p class="font-medium text-slate-900">{{ invoice.amount }}</p>
                <p class="text-slate-500">Due {{ invoice.dueDate }}</p>
              </div>
              <FileText class="h-4 w-4 text-slate-400" />
            </div>
          </div>
        </div>
      </div>
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <SectionHeader title="Payment History" subtitle="Recent payments received" />
        <div class="space-y-3">
          <div v-for="payment in payments" :key="payment.id" class="flex items-center justify-between gap-4 rounded-2xl border border-slate-200 p-4">
            <div class="flex items-start gap-3">
              <CheckCircle2 class="mt-0.5 h-4 w-4 text-green-500" />
              <div>
                <p class="text-sm font-medium text-slate-900">{{ payment.tenant }}</p>
                <p class="mt-1 text-xs text-slate-500">{{ payment.method }} • {{ payment.date }}</p>
              </div>
            </div>
            <div class="text-right">
              <p class="text-sm font-medium text-slate-900">{{ payment.amount }}</p>
              <p class="text-xs text-slate-500">{{ payment.invoice }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
})

const SchedulingCalendar = defineComponent({
  name: "SchedulingCalendar",
  components: { SectionHeader, CalendarDays, Clock3 },
  props: { scheduledTasks: { type: Array, required: true } },
  template: `
    <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
      <SectionHeader title="Upcoming Schedule" subtitle="Scheduled maintenance and inspections" />
      <div class="space-y-4">
        <div v-for="task in scheduledTasks" :key="task.id" class="rounded-2xl border border-slate-200 p-4">
          <div class="flex items-start justify-between gap-3">
            <div class="flex items-start gap-3">
              <CalendarDays class="mt-1 h-5 w-5 text-slate-400" />
              <div>
                <p class="text-sm font-semibold text-slate-950">{{ task.title }}</p>
                <p class="mt-1 text-xs text-slate-500">{{ task.facility }}</p>
                <div class="mt-2 flex items-center gap-4 text-xs text-slate-600">
                  <span class="inline-flex items-center gap-1">
                    <Clock3 class="h-3.5 w-3.5" />{{ task.date }} at {{ task.time }}
                  </span>
                  <span class="font-medium">{{ task.technician }}</span>
                </div>
              </div>
            </div>
            <span :class="task.type === 'Preventive' ? 'bg-blue-100 text-blue-700' : 'bg-purple-100 text-purple-700'" class="rounded-full px-2.5 py-1 text-xs font-medium">
              {{ task.type }}
            </span>
          </div>
        </div>
      </div>
    </div>
  `,
})

const CustomerPortal = defineComponent({
  name: "CustomerPortal",
  components: { SectionHeader, Users, CheckCircle2, Clock3 },
  props: { tenants: { type: Array, required: true }, portalActivity: { type: Array, required: true } },
  template: `
    <div class="grid gap-6 xl:grid-cols-[1fr_1fr]">
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <SectionHeader title="Tenant Directory" subtitle="Portal access and contact information" />
        <div class="space-y-3">
          <div v-for="tenant in tenants" :key="tenant.id" class="rounded-2xl border border-slate-200 p-4">
            <div class="flex items-start justify-between gap-3">
              <div>
                <p class="text-sm font-semibold text-slate-950">{{ tenant.name }}</p>
                <p class="mt-1 text-xs text-slate-500">{{ tenant.facility }} • {{ tenant.contact }}</p>
                <p class="text-xs text-slate-500">{{ tenant.email }}</p>
              </div>
              <div class="text-right">
                <span :class="tenant.portalAccess ? 'bg-green-100 text-green-700' : 'bg-gray-100 text-gray-700'" class="rounded-full px-2.5 py-1 text-xs font-medium mb-2 block">
                  {{ tenant.portalAccess ? 'Active' : 'Inactive' }}
                </span>
                <p class="text-xs text-slate-500">Last login: {{ tenant.lastLogin }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <SectionHeader title="Portal Activity" subtitle="Recent tenant interactions and requests" />
        <div class="space-y-3">
          <div v-for="activity in portalActivity" :key="activity.id" class="rounded-2xl border border-slate-200 p-4">
            <div class="flex items-start justify-between gap-3">
              <div>
                <p class="text-sm font-semibold text-slate-950">{{ activity.tenant }}</p>
                <p class="mt-1 text-xs text-slate-600">{{ activity.action }}</p>
                <p class="text-xs text-slate-500">{{ activity.details }}</p>
              </div>
              <div class="text-right">
                <span :class="activity.status === 'New' ? 'bg-blue-100 text-blue-700' : activity.status === 'Viewed' ? 'bg-yellow-100 text-yellow-700' : 'bg-green-100 text-green-700'" class="rounded-full px-2.5 py-1 text-xs font-medium mb-2 block">
                  {{ activity.status }}
                </span>
                <p class="text-xs text-slate-500">{{ activity.timestamp }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
})

const RecruitingBoard = defineComponent({
  name: "RecruitingBoard",
  components: { StagePill },
  props: {
    stages: { type: Array, required: true },
    recruits: { type: Array, required: true },
  },
  methods: {
    filtered(stageKey) {
      return this.recruits.filter((r) => r.stage === stageKey)
    },
  },
  template: `
    <div class="grid gap-4 xl:grid-cols-6">
      <div v-for="stage in stages" :key="stage.key" class="rounded-3xl border border-slate-200 bg-slate-50 p-4">
        <div class="mb-4 flex items-center justify-between">
          <div>
            <p class="text-sm font-semibold text-slate-900">{{ stage.label }}</p>
            <p class="text-xs text-slate-500">{{ filtered(stage.key).length }} candidates</p>
          </div>
          <span class="rounded-full bg-white px-2.5 py-1 text-xs font-medium text-slate-600">{{ filtered(stage.key).length }}</span>
        </div>
        <div class="space-y-3">
          <template v-if="filtered(stage.key).length">
            <div v-for="candidate in filtered(stage.key)" :key="candidate.id" class="rounded-2xl border border-slate-200 bg-white p-3 shadow-sm">
              <div class="flex items-start justify-between gap-2">
                <div>
                  <p class="text-sm font-semibold text-slate-950">{{ candidate.name }}</p>
                  <p class="mt-1 text-xs text-slate-500">{{ candidate.id }} • {{ candidate.state }}</p>
                </div>
                <span class="rounded-full bg-slate-100 px-2 py-1 text-[11px] font-medium text-slate-700">{{ candidate.fit }}</span>
              </div>
              <div class="mt-3 flex flex-wrap gap-2 text-[11px] text-slate-600">
                <StagePill>{{ candidate.licensing }}</StagePill>
                <StagePill>{{ candidate.availability }}</StagePill>
              </div>
              <p class="mt-3 text-xs leading-6 text-slate-600">{{ candidate.ai }}</p>
            </div>
          </template>
          <div v-else class="rounded-2xl border border-dashed border-slate-300 bg-white/70 p-4 text-xs text-slate-500">No candidates in this stage</div>
        </div>
      </div>
    </div>
  `,
})

const PipelineBoard = defineComponent({
  name: "PipelineBoard",
  components: { LeadCard },
  props: {
    stages: { type: Array, required: true },
    leads: { type: Array, required: true },
  },
  methods: {
    filtered(stageKey) {
      return this.leads.filter((lead) => lead.stage === stageKey)
    },
  },
  template: `
    <div class="grid gap-4 xl:grid-cols-6">
      <div v-for="stage in stages" :key="stage.key" class="rounded-3xl border border-slate-200 bg-slate-50 p-4">
        <div class="mb-4 flex items-center justify-between">
          <div>
            <p class="text-sm font-semibold text-slate-900">{{ stage.label }}</p>
            <p class="text-xs text-slate-500">{{ filtered(stage.key).length }} records</p>
          </div>
          <span class="rounded-full bg-white px-2.5 py-1 text-xs font-medium text-slate-600">{{ filtered(stage.key).length }}</span>
        </div>
        <div class="space-y-3">
          <template v-if="filtered(stage.key).length">
            <LeadCard v-for="lead in filtered(stage.key)" :key="lead.id" :lead="lead" />
          </template>
          <div v-else class="rounded-2xl border border-dashed border-slate-300 bg-white/70 p-4 text-xs text-slate-500">No leads in this stage</div>
        </div>
      </div>
    </div>
  `,
})

const IntakeModal = defineComponent({
  name: "IntakeModal",
  components: { Wand2, X },
  emits: ["close", "update:path", "update:summary"],
  props: {
    path: { type: String, required: true },
    summary: { type: String, required: true },
  },
  computed: {
    current() {
      return prompts[this.path]
    },
    jsonPreview() {
      return `{
  "record_type": "${
    this.path === "service"
      ? "work_order"
      : this.path === "proposal"
      ? "opportunity"
      : "recruit"
  }",
  "summary": "AI-generated summary goes here.",
  "priority": "${this.path === "service" ? "high" : "medium"}",
  "recommended_stage": "${
    this.path === "service"
      ? "AI Triaged"
      : this.path === "proposal"
      ? "AI Qualified"
      : "AI Pre-Qualified"
  }",
  "next_action": "Create follow-up task and assign owner",
  "confidence": 0.91,
  "missing_fields": []
}`
    },
  },
  methods: {
    setPath(value) {
      this.$emit("update:path", value)
    },
    setSummary(event) {
      this.$emit("update:summary", event.target.value)
    },
  },
  template: `
    <div class="fixed inset-0 z-50 flex items-center justify-center bg-slate-950/50 p-4 backdrop-blur-sm">
      <div class="grid max-h-[92vh] w-full max-w-6xl overflow-hidden rounded-[28px] border border-slate-200 bg-white shadow-2xl lg:grid-cols-[1.05fr_0.95fr]">
        <div class="overflow-y-auto p-6 lg:p-8">
          <div class="flex items-start justify-between gap-4">
            <div>
              <div class="inline-flex items-center gap-2 rounded-full bg-slate-100 px-3 py-1 text-xs font-semibold text-slate-700">
                <Wand2 class="h-3.5 w-3.5" /> AI Intake Modal
              </div>
              <h2 class="mt-4 text-2xl font-semibold text-slate-950">{{ current.title }}</h2>
              <p class="mt-2 text-sm leading-6 text-slate-500">{{ current.helper }}</p>
            </div>
            <button @click="$emit('close')" class="rounded-2xl border border-slate-200 p-2 text-slate-500 hover:bg-slate-50">
              <X class="h-4 w-4" />
            </button>
          </div>

          <div class="mt-6 flex flex-wrap gap-2">
            <button
              v-for="item in [
                { id: 'service', label: 'Service Request' },
                { id: 'proposal', label: 'Proposal' },
                { id: 'recruit', label: 'Recruiting' },
              ]"
              :key="item.id"
              @click="setPath(item.id)"
              :class="path === item.id ? 'bg-slate-950 text-white' : 'bg-slate-100 text-slate-700 hover:bg-slate-200'"
              class="rounded-full px-4 py-2 text-sm font-medium transition"
            >
              {{ item.label }}
            </button>
          </div>

          <div class="mt-8 grid gap-4 md:grid-cols-2">
            <label v-for="field in current.fields" :key="field" class="block">
              <span class="mb-2 block text-sm font-medium text-slate-700">{{ field }}</span>
              <input class="w-full rounded-2xl border border-slate-300 bg-slate-50 px-4 py-3 text-sm outline-none focus:border-slate-500" :placeholder="'Enter ' + field.toLowerCase()" />
            </label>
          </div>

          <div class="mt-6">
            <label class="block">
              <span class="mb-2 block text-sm font-medium text-slate-700">Freeform summary</span>
              <textarea
                :value="summary"
                @input="setSummary"
                class="min-h-[120px] w-full rounded-2xl border border-slate-300 bg-slate-50 px-4 py-3 text-sm outline-none focus:border-slate-500"
                placeholder="Paste notes, explain the issue, or describe what the person is asking for..."
              />
            </label>
          </div>

          <div class="mt-6 flex flex-wrap gap-3">
            <button class="rounded-2xl bg-slate-950 px-5 py-3 text-sm font-medium text-white">Run AI Intake</button>
            <button class="rounded-2xl border border-slate-300 px-5 py-3 text-sm font-medium text-slate-700">Save Draft</button>
          </div>
        </div>

        <div class="overflow-y-auto border-l border-slate-200 bg-slate-50 p-6 lg:p-8">
          <h3 class="text-lg font-semibold text-slate-950">Prompt + workflow preview</h3>
          <div class="mt-5 rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
            <p class="text-xs font-semibold uppercase tracking-[0.16em] text-slate-500">System prompt</p>
            <p class="mt-3 text-sm leading-7 text-slate-700">{{ current.system }}</p>
          </div>

          <div class="mt-5 rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
            <p class="text-xs font-semibold uppercase tracking-[0.16em] text-slate-500">Workflow output</p>
            <div class="mt-3 space-y-3 text-sm text-slate-700">
              <div class="flex items-start gap-3"><span class="mt-0.5 rounded-full bg-slate-100 px-2 py-1 text-xs font-medium">1</span><p>Normalize intake into a structured record.</p></div>
              <div class="flex items-start gap-3"><span class="mt-0.5 rounded-full bg-slate-100 px-2 py-1 text-xs font-medium">2</span><p>Score urgency, fit, or opportunity value.</p></div>
              <div class="flex items-start gap-3"><span class="mt-0.5 rounded-full bg-slate-100 px-2 py-1 text-xs font-medium">3</span><p>Assign pipeline, stage, and recommended owner queue.</p></div>
              <div class="flex items-start gap-3"><span class="mt-0.5 rounded-full bg-slate-100 px-2 py-1 text-xs font-medium">4</span><p>Create follow-up tasks and AI summary for review.</p></div>
            </div>
          </div>

          <div class="mt-5 rounded-3xl border border-slate-200 bg-slate-950 p-5 text-white shadow-sm">
            <p class="text-xs font-semibold uppercase tracking-[0.16em] text-slate-400">Structured JSON example</p>
            <pre class="mt-3 overflow-x-auto whitespace-pre-wrap text-xs leading-6 text-slate-200">{{ jsonPreview }}</pre>
          </div>
        </div>
      </div>
    </div>
  `,
})
</script>
