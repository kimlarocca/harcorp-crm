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
              <p class="text-base font-semibold">Harcorp Project Management</p>
              <p class="text-xs text-slate-400">Property Operations Platform</p>
            </div>
          </div>
        </div>

        <div class="px-4 py-5">
          <button
            @click="createNewProject"
            class="flex w-full items-center justify-center gap-2 rounded-2xl bg-white px-4 py-3 text-sm font-semibold text-slate-950 shadow-sm transition hover:-translate-y-0.5"
          >
            <Plus class="h-4 w-4" /> New Project
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
                  label="Active Projects"
                  :value="projects.filter(p => p.status === 'Active').length.toString()"
                  :icon="Building2"
                  :meta="`${projects.filter(p => p.status === 'Maintenance').length} under maintenance`"
                />
                <StatCard
                  label="Open Work Orders"
                  :value="workOrders.length.toString()"
                  :icon="Wrench"
                  :meta="`${workOrders.filter(wo => wo.urgency === 'Emergency').length} urgent, ${workOrders.filter(wo => wo.urgency !== 'Emergency').length} routine`"
                />
                <StatCard
                  label="Monthly Revenue"
                  :value="`$${invoices.filter(inv => inv.status === 'Paid').reduce((sum, inv) => sum + parseFloat(inv.amount.replace(/[$,]/g, '')), 0).toLocaleString()}`"
                  :icon="FileText"
                  :meta="`${invoices.filter(inv => inv.status === 'Paid').length} invoices paid`"
                />
                <StatCard
                  label="Scheduled Tasks"
                  :value="scheduledTasks.length.toString()"
                  :icon="CalendarDays"
                  :meta="`${scheduledTasks.filter(task => new Date(task.date).toDateString() === new Date().toDateString()).length} due today`"
                />
              </div>

              <div class="grid gap-6 xl:grid-cols-[1.35fr_0.95fr]">
                <div
                  class="rounded-[28px] border border-slate-200 bg-white p-5 shadow-sm"
                >
                  <SectionHeader
                    title="Project Summary"
                    subtitle="Overview of all managed properties"
                  >
                    <template #action>
                      <button
                        @click="createNewProject"
                        class="rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                      >
                        Add Project
                      </button>
                    </template>
                  </SectionHeader>
                  <div class="space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                      <div class="rounded-lg bg-blue-50 p-4">
                        <div class="flex items-center gap-2">
                          <Building2 class="h-5 w-5 text-blue-600" />
                          <span class="text-sm font-medium text-blue-900">Active Projects</span>
                        </div>
                        <p class="mt-2 text-2xl font-bold text-blue-900">{{ projects.filter(p => p.status === 'Active').length }}</p>
                      </div>
                      <div class="rounded-lg bg-yellow-50 p-4">
                        <div class="flex items-center gap-2">
                          <Wrench class="h-5 w-5 text-yellow-600" />
                          <span class="text-sm font-medium text-yellow-900">Under Maintenance</span>
                        </div>
                        <p class="mt-2 text-2xl font-bold text-yellow-900">{{ projects.filter(p => p.status === 'Maintenance').length }}</p>
                      </div>
                    </div>
                    <div class="space-y-2">
                      <h4 class="text-sm font-medium text-slate-900">Recent Projects</h4>
                      <div class="space-y-2">
                        <div v-for="project in projects.slice(0, 3)" :key="project.id" 
                             class="flex items-center justify-between rounded-lg border border-slate-200 p-3">
                          <div>
                            <p class="text-sm font-medium text-slate-900">{{ project.name }}</p>
                            <p class="text-xs text-slate-500">{{ project.address }}</p>
                          </div>
                          <span :class="project.status === 'Active' ? 'bg-green-100 text-green-700' : 'bg-yellow-100 text-yellow-700'" 
                                class="rounded-full px-2 py-1 text-xs font-medium">
                            {{ project.status }}
                          </span>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <div class="rounded-[28px] border border-slate-200 bg-white p-5 shadow-sm">
                  <SectionHeader
                    title="Scheduled Tasks"
                    subtitle="Upcoming maintenance and inspections"
                  />
                  <div class="space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                      <div class="rounded-lg bg-green-50 p-4">
                        <div class="flex items-center gap-2">
                          <CalendarDays class="h-5 w-5 text-green-600" />
                          <span class="text-sm font-medium text-green-900">Total Tasks</span>
                        </div>
                        <p class="mt-2 text-2xl font-bold text-green-900">{{ scheduledTasks.length }}</p>
                      </div>
                      <div class="rounded-lg bg-orange-50 p-4">
                        <div class="flex items-center gap-2">
                          <Clock3 class="h-5 w-5 text-orange-600" />
                          <span class="text-sm font-medium text-orange-900">Due Today</span>
                        </div>
                        <p class="mt-2 text-2xl font-bold text-orange-900">
                          {{ scheduledTasks.filter(task => new Date(task.date).toDateString() === new Date().toDateString()).length }}
                        </p>
                      </div>
                    </div>
                    <div class="space-y-2">
                      <h4 class="text-sm font-medium text-slate-900">Upcoming Tasks</h4>
                      <div class="space-y-2">
                        <div v-for="task in scheduledTasks.slice(0, 3)" :key="task.id" 
                             class="flex items-center justify-between rounded-lg border border-slate-200 p-3">
                          <div>
                            <p class="text-sm font-medium text-slate-900">{{ task.title }}</p>
                            <p class="text-xs text-slate-500">{{ task.date }} at {{ task.time }}</p>
                          </div>
                          <span :class="task.type === 'Preventive' ? 'bg-blue-100 text-blue-700' : 'bg-purple-100 text-purple-700'" 
                                class="rounded-full px-2 py-1 text-xs font-medium">
                            {{ task.type }}
                          </span>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="grid gap-6 xl:grid-cols-[1fr_1fr]">
                <div class="rounded-[28px] border border-slate-200 bg-white p-5 shadow-sm">
                  <SectionHeader
                    title="Work Orders Summary"
                    subtitle="Current maintenance and repair tasks"
                  />
                  <div class="space-y-4">
                    <div class="grid grid-cols-3 gap-4">
                      <div class="rounded-lg bg-red-50 p-4">
                        <span class="text-sm font-medium text-red-900">Emergency</span>
                        <p class="mt-2 text-2xl font-bold text-red-900">
                          {{ workOrders.filter(wo => wo.urgency === 'Emergency').length }}
                        </p>
                      </div>
                      <div class="rounded-lg bg-orange-50 p-4">
                        <span class="text-sm font-medium text-orange-900">High Priority</span>
                        <p class="mt-2 text-2xl font-bold text-orange-900">
                          {{ workOrders.filter(wo => wo.urgency === 'High').length }}
                        </p>
                      </div>
                      <div class="rounded-lg bg-blue-50 p-4">
                        <span class="text-sm font-medium text-blue-900">Standard</span>
                        <p class="mt-2 text-2xl font-bold text-blue-900">
                          {{ workOrders.filter(wo => wo.urgency === 'Standard').length }}
                        </p>
                      </div>
                    </div>
                    <div class="space-y-2">
                      <h4 class="text-sm font-medium text-slate-900">Recent Work Orders</h4>
                      <div class="space-y-2">
                        <div v-for="wo in workOrders.slice(0, 3)" :key="wo.id" 
                             class="flex items-center justify-between rounded-lg border border-slate-200 p-3">
                          <div>
                            <p class="text-sm font-medium text-slate-900">{{ wo.id }}</p>
                            <p class="text-xs text-slate-500">{{ wo.client }} • {{ wo.category }}</p>
                          </div>
                          <span :class="wo.urgency === 'Emergency' ? 'bg-red-100 text-red-700' : wo.urgency === 'High' ? 'bg-orange-100 text-orange-700' : 'bg-blue-100 text-blue-700'" 
                                class="rounded-full px-2 py-1 text-xs font-medium">
                            {{ wo.urgency }}
                          </span>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <div class="rounded-[28px] border border-slate-200 bg-white p-5 shadow-sm">
                  <SectionHeader
                    title="Billing Overview"
                    subtitle="Invoice status and payment tracking"
                  />
                  <div class="space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                      <div class="rounded-lg bg-green-50 p-4">
                        <div class="flex items-center gap-2">
                          <FileText class="h-5 w-5 text-green-600" />
                          <span class="text-sm font-medium text-green-900">Paid Invoices</span>
                        </div>
                        <p class="mt-2 text-2xl font-bold text-green-900">
                          {{ invoices.filter(inv => inv.status === 'Paid').length }}
                        </p>
                      </div>
                      <div class="rounded-lg bg-yellow-50 p-4">
                        <div class="flex items-center gap-2">
                          <Clock3 class="h-5 w-5 text-yellow-600" />
                          <span class="text-sm font-medium text-yellow-900">Pending</span>
                        </div>
                        <p class="mt-2 text-2xl font-bold text-yellow-900">
                          {{ invoices.filter(inv => inv.status === 'Pending').length }}
                        </p>
                      </div>
                    </div>
                    <div class="space-y-2">
                      <h4 class="text-sm font-medium text-slate-900">Recent Invoices</h4>
                      <div class="space-y-2">
                        <div v-for="invoice in invoices.slice(0, 3)" :key="invoice.id" 
                             class="flex items-center justify-between rounded-lg border border-slate-200 p-3">
                          <div>
                            <p class="text-sm font-medium text-slate-900">{{ invoice.id }}</p>
                            <p class="text-xs text-slate-500">{{ invoice.tenant }} • {{ invoice.amount }}</p>
                          </div>
                          <span :class="invoice.status === 'Paid' ? 'bg-green-100 text-green-700' : invoice.status === 'Pending' ? 'bg-yellow-100 text-yellow-700' : 'bg-red-100 text-red-700'" 
                                class="rounded-full px-2 py-1 text-xs font-medium">
                            {{ invoice.status }}
                          </span>
                        </div>
                      </div>
                    </div>
                    <div class="pt-4 border-t border-slate-200">
                      <div class="flex items-center justify-between">
                        <span class="text-sm font-medium text-slate-900">Monthly Revenue</span>
                        <span class="text-lg font-bold text-slate-900">
                          ${{ invoices.filter(inv => inv.status === 'Paid').reduce((sum, inv) => sum + parseFloat(inv.amount.replace(/[$,]/g, '')), 0).toLocaleString() }}
                        </span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </template>

          <template v-else-if="activeView === 'projects'">
            <div class="space-y-6">
              <SectionHeader
                title="Project Management"
                subtitle="Monitor and manage all properties in your portfolio"
              >
                <template #action>
                  <button
                    @click="createNewProject"
                    class="rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                  >
                    Add Project
                  </button>
                </template>
              </SectionHeader>
              <ProjectsGrid :projects="projects" />
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

          <template v-else-if="activeView === 'follow-up'">
            <div class="space-y-6">
              <SectionHeader
                title="Follow-Up Assistant"
                subtitle="Track inactive tasks and manage follow-ups"
              >
                <template #action>
                  <button
                    class="inline-flex items-center gap-2 rounded-2xl bg-slate-950 px-4 py-2 text-sm font-medium text-white"
                  >
                    <Filter class="h-4 w-4" /> Filter Tasks
                  </button>
                </template>
              </SectionHeader>
              <FollowUpDashboard :workOrders="workOrders" :scheduledTasks="scheduledTasks" />
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
                subtitle="Intelligent agents helping with project management workflows"
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
                    title="Project Management Agents"
                    subtitle="AI-powered assistance for maintenance, billing, and operations"
                  />
                  <div class="space-y-4">
                    <div
                      v-for="item in projectAgentPrompts"
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
  ChevronLeft,
  X,
  Wand2,
} from "lucide-vue-next"

const cx = (...classes) => classes.filter(Boolean).join(" ")

const navItems = [
  { id: "dashboard", label: "Dashboard", icon: LayoutDashboard },
  { id: "projects", label: "Projects", icon: Building2 },
  { id: "maintenance", label: "Maintenance", icon: Wrench },
  { id: "follow-up", label: "Follow-Up", icon: ClipboardList },
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
    name: "Montgomery County Projects",
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
    org: "Montgomery County Projects",
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
    title: "Projects Director",
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
    lastActivity: new Date(Date.now() - 2 * 60 * 60 * 1000).toISOString(), // 2 hours ago
    assignee: "John Smith"
  },
  {
    id: "HC23-2896",
    client: "Montgomery County Projects",
    category: "Preventive Maintenance",
    status: "Awaiting Review",
    urgency: "Standard",
    sla: "Due tomorrow",
    asset: "Boiler inspection",
    lastActivity: new Date(Date.now() - 48 * 60 * 60 * 1000).toISOString(), // 48 hours ago
    assignee: "Sarah Davis"
  },
  {
    id: "HC23-2898",
    client: "PA National Guard Support",
    category: "Kitchen Equipment",
    status: "In Progress",
    urgency: "High",
    sla: "On track",
    asset: "Steam kettle diagnosis",
    lastActivity: new Date(Date.now() - 72 * 60 * 60 * 1000).toISOString(), // 72 hours ago
    assignee: "Mike Johnson"
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
  { id: "WO-2024-0456", title: "HVAC System Maintenance", project: "Downtown Office", completed: "2 days ago", cost: "$2,450", technician: "Mike Johnson" },
  { id: "WO-2024-0455", title: "Plumbing Repair - Floor 3", project: "Medical Plaza", completed: "3 days ago", cost: "$890", technician: "Sarah Davis" },
  { id: "WO-2024-0454", title: "Electrical Panel Upgrade", project: "Warehouse A", completed: "1 week ago", cost: "$3,200", technician: "Tom Wilson" },
  { id: "WO-2024-0453", title: "Roof Leak Repair", project: "Retail Center", completed: "1 week ago", cost: "$1,150", technician: "Lisa Chen" },
]

const projects = ref([
  { 
    id: "FAC-001", 
    name: "Downtown Office Complex", 
    address: "123 Main St, Downtown", 
    status: "Active", 
    type: "Office", 
    sqFt: "50,000", 
    tenants: 12, 
    lastInspection: "2 weeks ago",
    date: "2024-01-15",
    website: "https://downtownoffice.com",
    cost: "$2,500,000",
    contactName: "John Smith",
    contactNumber: "(555) 123-4567",
    contactEmail: "john@techcorp.com",
    source: "Direct"
  },
  { 
    id: "FAC-002", 
    name: "Medical Plaza", 
    address: "456 Health Ave, Medical District", 
    status: "Active", 
    type: "Medical", 
    sqFt: "75,000", 
    tenants: 8, 
    lastInspection: "1 week ago",
    date: "2024-02-20",
    website: "https://medicalplaza.com",
    cost: "$4,200,000",
    contactName: "Dr. Sarah Johnson",
    contactNumber: "(555) 234-5678",
    contactEmail: "sarah@medicare.com",
    source: "Referral"
  },
  { 
    id: "FAC-003", 
    name: "Warehouse A", 
    address: "789 Industrial Blvd", 
    status: "Maintenance", 
    type: "Warehouse", 
    sqFt: "100,000", 
    tenants: 2, 
    lastInspection: "3 days ago",
    date: "2024-03-10",
    website: "https://warehouse-a.com",
    cost: "$1,800,000",
    contactName: "Mike Davis",
    contactNumber: "(555) 345-6789",
    contactEmail: "mike@logistics.com",
    source: "Online"
  },
  { 
    id: "FAC-004", 
    name: "Retail Center", 
    address: "321 Shopping Mall Dr", 
    status: "Active", 
    type: "Retail", 
    sqFt: "40,000", 
    tenants: 15, 
    lastInspection: "1 month ago",
    date: "2024-04-05",
    website: "https://retailcenter.com",
    cost: "$3,100,000",
    contactName: "Lisa Chen",
    contactNumber: "(555) 456-7890",
    contactEmail: "lisa@retail.com",
    source: "Partnership"
  },
])

const createNewProject = () => {
  const newId = `PROJ-${String(projects.value.length + 1).padStart(3, '0')}`
  const newProject = {
    id: newId,
    name: `New Project ${projects.value.length + 1}`,
    address: "Address TBD",
    status: "Active",
    type: "Office",
    sqFt: "0",
    tenants: 0,
    lastInspection: "Never",
    date: new Date().toISOString().split('T')[0], // Today's date in YYYY-MM-DD format
    website: "",
    cost: "$0",
    contactName: "",
    contactNumber: "",
    contactEmail: "",
    source: "Direct"
  }
  projects.value.push(newProject)
  activeView.value = 'projects'
}

const invoices = [
  { id: "INV-2024-001", tenant: "TechCorp Inc", project: "Downtown Office", amount: "$12,500", dueDate: "2024-04-15", status: "Paid", issued: "2024-03-15" },
  { id: "INV-2024-002", tenant: "MediCare Plus", project: "Medical Plaza", amount: "$8,750", dueDate: "2024-04-20", status: "Pending", issued: "2024-03-20" },
  { id: "INV-2024-003", tenant: "Logistics Pro", project: "Warehouse A", amount: "$15,200", dueDate: "2024-04-10", status: "Overdue", issued: "2024-03-10" },
  { id: "INV-2024-004", tenant: "Retail Group", project: "Retail Center", amount: "$6,800", dueDate: "2024-04-25", status: "Paid", issued: "2024-03-25" },
]

const payments = [
  { id: "PAY-2024-001", tenant: "TechCorp Inc", amount: "$12,500", method: "ACH", date: "2024-04-10", invoice: "INV-2024-001" },
  { id: "PAY-2024-002", tenant: "MediCare Plus", amount: "$4,375", method: "Check", date: "2024-04-12", invoice: "INV-2024-002" },
  { id: "PAY-2024-003", tenant: "Retail Group", amount: "$6,800", method: "Wire", date: "2024-04-08", invoice: "INV-2024-004" },
]

const scheduledTasks = [
  { id: "SCH-001", title: "HVAC Filter Replacement", project: "Downtown Office", date: "2024-04-25", time: "9:00 AM", technician: "Mike Johnson", type: "Preventive", lastActivity: new Date(Date.now() - 24 * 60 * 60 * 1000).toISOString(), assignee: "Mike Johnson" },
  { id: "SCH-002", title: "Fire Alarm Inspection", project: "Medical Plaza", date: "2024-04-26", time: "2:00 PM", technician: "Sarah Davis", type: "Inspection", lastActivity: new Date(Date.now() - 48 * 60 * 60 * 1000).toISOString(), assignee: "Sarah Davis" },
  { id: "SCH-003", title: "Elevator Maintenance", project: "Retail Center", date: "2024-04-28", time: "8:00 AM", technician: "Tom Wilson", type: "Preventive", lastActivity: new Date(Date.now() - 72 * 60 * 60 * 1000).toISOString(), assignee: "Tom Wilson" },
  { id: "SCH-004", title: "Plumbing Check", project: "Warehouse A", date: "2024-04-30", time: "10:00 AM", technician: "Lisa Chen", type: "Inspection", lastActivity: new Date(Date.now() - 5 * 60 * 1000).toISOString(), assignee: "Lisa Chen" },
]

const tenants = [
  { id: "TEN-001", name: "TechCorp Inc", project: "Downtown Office", contact: "John Smith", email: "john@techcorp.com", portalAccess: true, lastLogin: "2 days ago" },
  { id: "TEN-002", name: "MediCare Plus", project: "Medical Plaza", contact: "Dr. Sarah Johnson", email: "sarah@medicare.com", portalAccess: true, lastLogin: "1 day ago" },
  { id: "TEN-003", name: "Logistics Pro", project: "Warehouse A", contact: "Mike Davis", email: "mike@logistics.com", portalAccess: false, lastLogin: "Never" },
  { id: "TEN-004", name: "Retail Group", project: "Retail Center", contact: "Lisa Chen", email: "lisa@retail.com", portalAccess: true, lastLogin: "5 hours ago" },
]

const portalActivity = [
  { id: "ACT-001", tenant: "TechCorp Inc", action: "Submitted service request", details: "HVAC issue in Suite 1201", timestamp: "2 hours ago", status: "New" },
  { id: "ACT-002", tenant: "MediCare Plus", action: "Viewed invoice", details: "Invoice INV-2024-002", timestamp: "4 hours ago", status: "Viewed" },
  { id: "ACT-003", tenant: "Retail Group", action: "Updated contact info", details: "Changed phone number", timestamp: "1 day ago", status: "Completed" },
  { id: "ACT-004", tenant: "TechCorp Inc", action: "Downloaded lease", details: "Lease agreement renewal", timestamp: "2 days ago", status: "Downloaded" },
]

const projectAgentPrompts = [
  {
    title: "Maintenance Agent",
    body: "Analyze maintenance requests, prioritize by urgency and impact, suggest optimal scheduling, and recommend preventive measures based on historical data.",
  },
  {
    title: "Billing Agent",
    body: "Generate accurate invoices, track payment status, send automated reminders, and provide financial reporting for project operations.",
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
              <p class="mt-1 text-xs text-slate-500">{{ item.project }} • {{ item.completed }}</p>
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

const ProjectsGrid = defineComponent({
  name: "ProjectsGrid",
  components: { SectionHeader, Building2, MapPin },
  props: { projects: { type: Array, required: true } },
  template: `
    <div class="grid gap-4 md:grid-cols-2 xl:grid-cols-3">
      <div v-for="project in projects" :key="project.id" class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <div class="flex items-start justify-between gap-3">
          <div class="flex items-start gap-3">
            <Building2 class="mt-1 h-5 w-5 text-slate-400" />
            <div>
              <p class="text-sm font-semibold text-slate-950">{{ project.name }}</p>
              <p class="mt-1 text-xs text-slate-500 flex items-center gap-1">
                <MapPin class="h-3 w-3" />{{ project.address }}
              </p>
            </div>
          </div>
          <span :class="project.status === 'Active' ? 'bg-green-100 text-green-700' : 'bg-yellow-100 text-yellow-700'" class="rounded-full px-2.5 py-1 text-xs font-medium">
            {{ project.status }}
          </span>
        </div>
        <div class="mt-4 grid grid-cols-2 gap-4 text-sm">
          <div>
            <p class="text-slate-500">Type</p>
            <p class="font-medium text-slate-900">{{ project.type }}</p>
          </div>
          <div>
            <p class="text-slate-500">Sq Ft</p>
            <p class="font-medium text-slate-900">{{ project.sqFt }}</p>
          </div>
          <div>
            <p class="text-slate-500">Cost</p>
            <p class="font-medium text-slate-900">{{ project.cost }}</p>
          </div>
          <div>
            <p class="text-slate-500">Date</p>
            <p class="font-medium text-slate-900">{{ project.date }}</p>
          </div>
          <div>
            <p class="text-slate-500">Contact</p>
            <p class="font-medium text-slate-900">{{ project.contactName }}</p>
          </div>
          <div>
            <p class="text-slate-500">Source</p>
            <p class="font-medium text-slate-900">{{ project.source }}</p>
          </div>
        </div>
        <div class="mt-4 space-y-2 text-xs">
          <div v-if="project.website" class="flex items-center gap-2">
            <span class="text-slate-500">Website:</span>
            <a :href="project.website" target="_blank" class="text-blue-600 hover:text-blue-800">{{ project.website }}</a>
          </div>
          <div v-if="project.contactEmail" class="flex items-center gap-2">
            <span class="text-slate-500">Email:</span>
            <a :href="'mailto:' + project.contactEmail" class="text-blue-600 hover:text-blue-800">{{ project.contactEmail }}</a>
          </div>
          <div v-if="project.contactNumber" class="flex items-center gap-2">
            <span class="text-slate-500">Phone:</span>
            <a :href="'tel:' + project.contactNumber" class="text-blue-600 hover:text-blue-800">{{ project.contactNumber }}</a>
          </div>
        </div>
      </div>
    </div>
  `,
})

const FollowUpDashboard = defineComponent({
  name: "FollowUpDashboard",
  components: { SectionHeader, Clock3, AlertCircle: Sparkles, CheckCircle2 },
  props: { workOrders: { type: Array, required: true }, scheduledTasks: { type: Array, required: true } },
  setup(props) {
    const calculateHoursSinceActivity = (lastActivityIso) => {
      const lastActivity = new Date(lastActivityIso)
      const now = new Date()
      return Math.floor((now - lastActivity) / (1000 * 60 * 60))
    }

    const getInactivityStage = (hours) => {
      if (hours < 24) return "24h"
      if (hours < 48) return "48h"
      return "72h+"
    }

    const allTasks = computed(() => {
      const tasks = []
      
      props.workOrders.forEach(wo => {
        tasks.push({
          ...wo,
          type: "Work Order",
          hoursInactive: calculateHoursSinceActivity(wo.lastActivity),
          stage: getInactivityStage(calculateHoursSinceActivity(wo.lastActivity))
        })
      })
      
      props.scheduledTasks.forEach(task => {
        tasks.push({
          ...task,
          type: "Scheduled Task",
          hoursInactive: calculateHoursSinceActivity(task.lastActivity),
          stage: getInactivityStage(calculateHoursSinceActivity(task.lastActivity)),
          client: task.project
        })
      })
      
      return tasks.sort((a, b) => b.hoursInactive - a.hoursInactive)
    })

    const tasksByStage = computed(() => ({
      "24h": allTasks.value.filter(t => t.stage === "24h"),
      "48h": allTasks.value.filter(t => t.stage === "48h"),
      "72h+": allTasks.value.filter(t => t.stage === "72h+")
    }))

    const stageConfig = {
      "24h": { color: "bg-blue-50", badge: "bg-blue-100 text-blue-700", icon: "text-blue-600", description: "Light nudge - just getting started" },
      "48h": { color: "bg-yellow-50", badge: "bg-yellow-100 text-yellow-700", icon: "text-yellow-600", description: "Increase urgency - prepare external touchpoint" },
      "72h+": { color: "bg-red-50", badge: "bg-red-100 text-red-700", icon: "text-red-600", description: "Stalled - immediate action needed" }
    }

    const formatHours = (hours) => {
      if (hours < 24) return `${hours}h ago`
      if (hours < 48) return `${Math.floor(hours / 24)}d ago`
      return `${Math.floor(hours / 24)}d ago`
    }

    return {
      allTasks,
      tasksByStage,
      stageConfig,
      formatHours,
      calculateHoursSinceActivity
    }
  },
  template: `
    <div class="space-y-6">
      <!-- Follow-Up Status Overview -->
      <div class="grid gap-4 md:grid-cols-3">
        <div class="rounded-lg bg-blue-50 border border-blue-200 p-4">
          <div class="flex items-center gap-2 mb-2">
            <Clock3 class="h-5 w-5 text-blue-600" />
            <span class="text-sm font-medium text-blue-900">24h Stage</span>
          </div>
          <p class="text-3xl font-bold text-blue-900">{{ tasksByStage['24h'].length }}</p>
          <p class="text-xs text-blue-700 mt-1">Light nudges</p>
        </div>
        <div class="rounded-lg bg-yellow-50 border border-yellow-200 p-4">
          <div class="flex items-center gap-2 mb-2">
            <Clock3 class="h-5 w-5 text-yellow-600" />
            <span class="text-sm font-medium text-yellow-900">48h Stage</span>
          </div>
          <p class="text-3xl font-bold text-yellow-900">{{ tasksByStage['48h'].length }}</p>
          <p class="text-xs text-yellow-700 mt-1">Stronger reminders</p>
        </div>
        <div class="rounded-lg bg-red-50 border border-red-200 p-4">
          <div class="flex items-center gap-2 mb-2">
            <Sparkles class="h-5 w-5 text-red-600" />
            <span class="text-sm font-medium text-red-900">72h+ Stage</span>
          </div>
          <p class="text-3xl font-bold text-red-900">{{ tasksByStage['72h+'].length }}</p>
          <p class="text-xs text-red-700 mt-1">Stalled items</p>
        </div>
      </div>

      <!-- Tasks by Stage -->
      <div class="space-y-6">
        <div v-for="stage in ['24h', '48h', '72h+']" :key="stage" class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
          <div class="mb-4">
            <h3 class="text-lg font-semibold text-slate-900">{{ stage }} Follow-Up Stage</h3>
            <p class="text-sm text-slate-600 mt-1">{{ stageConfig[stage].description }}</p>
          </div>
          
          <div v-if="tasksByStage[stage].length === 0" class="text-center py-8">
            <CheckCircle2 class="h-8 w-8 text-green-500 mx-auto mb-2" />
            <p class="text-sm text-slate-600">No tasks in this stage</p>
          </div>

          <div v-else class="space-y-3">
            <div v-for="task in tasksByStage[stage]" :key="task.id" 
                 :class="[stageConfig[stage].color, 'rounded-lg border border-slate-200 p-4']">
              <div class="flex items-start justify-between gap-3">
                <div class="flex-1">
                  <div class="flex items-center gap-2 mb-1">
                    <p class="font-semibold text-slate-900">{{ task.id }}</p>
                    <span :class="stageConfig[stage].badge" class="rounded-full px-2 py-0.5 text-xs font-medium">
                      {{ stage }}
                    </span>
                  </div>
                  <p class="text-sm text-slate-700 font-medium">{{ task.title || task.asset || task.category }}</p>
                  <p class="text-xs text-slate-600 mt-1">
                    {{ task.client }} • {{ task.type }}
                  </p>
                  <div class="flex items-center gap-4 mt-2 text-xs text-slate-600">
                    <span>Last activity: {{ formatHours(task.hoursInactive) }}</span>
                    <span>Assigned to: {{ task.assignee || task.technician }}</span>
                  </div>
                </div>
                <div class="text-right">
                  <div :class="[stageConfig[stage].badge, 'rounded-lg px-3 py-2 text-xs font-medium']">
                    {{ task.urgency || task.type }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
})

const MaintenanceBoard = defineComponent({
  name: "MaintenanceBoard",
  components: { SectionHeader, PriorityBadge, StagePill, Clock3, CheckCircle2, AlertCircle: Sparkles, Wrench, CalendarDays },
  props: { workOrders: { type: Array, required: true }, maintenanceHistory: { type: Array, required: true } },
  setup(props) {
    // Maintenance Agent Logic
    const checkSLACompliance = (workOrder) => {
      const slaText = workOrder.sla.toLowerCase()
      const urgency = workOrder.urgency.toLowerCase()
      
      if (urgency === 'emergency' && slaText.includes('2h')) {
        return { compliant: true, status: 'On Track', message: 'Emergency SLA met' }
      }
      if (urgency === 'high' && slaText.includes('today')) {
        return { compliant: true, status: 'On Track', message: 'High priority SLA met' }
      }
      if (urgency === 'standard' && slaText.includes('tomorrow')) {
        return { compliant: true, status: 'On Track', message: 'Standard SLA met' }
      }
      return { compliant: false, status: 'At Risk', message: 'SLA deadline approaching' }
    }

    const checkScheduleConflicts = (workOrders) => {
      const conflicts = []
      const technicianSchedule = {}
      
      workOrders.forEach(wo => {
        if (!technicianSchedule[wo.assignee]) {
          technicianSchedule[wo.assignee] = []
        }
        technicianSchedule[wo.assignee].push(wo)
      })
      
      Object.entries(technicianSchedule).forEach(([technician, tasks]) => {
        if (tasks.length > 1) {
          conflicts.push({
            technician,
            tasks: tasks.map(t => t.id),
            issue: 'Multiple assignments detected'
          })
        }
      })
      
      return conflicts
    }

    const generateWorkOrder = (workOrder) => {
      return `WO-${workOrder.client.replace(/\s+/g, '-').toUpperCase()}-${workOrder.asset.replace(/\s+/g, '-').toUpperCase()}-${workOrder.id}`
    }

    const slaStatus = computed(() => 
      props.workOrders.map(wo => ({
        ...wo,
        slaCheck: checkSLACompliance(wo)
      }))
    )

    const scheduleConflicts = computed(() => checkScheduleConflicts(props.workOrders))

    const workOrderRecommendations = computed(() => 
      props.workOrders.map(wo => ({
        ...wo,
        workOrderId: generateWorkOrder(wo),
        recommendations: [
          wo.status === 'Ready for Dispatch' ? 'Parts and tools verified - ready to dispatch' : 'Verify parts availability before dispatch',
          wo.slaCheck?.status === 'At Risk' ? 'SLA deadline approaching - prioritize scheduling' : 'SLA compliant',
          scheduleConflicts.value.some(c => c.tasks.includes(wo.id)) ? 'Schedule conflict detected - coordinate with technician' : 'No scheduling conflicts'
        ].filter(Boolean)
      }))
    )

    return {
      slaStatus,
      scheduleConflicts,
      workOrderRecommendations,
      checkSLACompliance
    }
  },
  template: `
    <div class="space-y-6">
      <!-- Maintenance Agent Overview -->
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <SectionHeader title="Maintenance Agent Dashboard" subtitle="AI-powered coordination and dispatch" />
        <div class="grid gap-4 md:grid-cols-3">
          <div class="rounded-lg bg-green-50 p-4">
            <div class="flex items-center gap-2">
              <CheckCircle2 class="h-5 w-5 text-green-600" />
              <span class="text-sm font-medium text-green-900">SLA Compliant</span>
            </div>
            <p class="mt-2 text-2xl font-bold text-green-900">
              {{ slaStatus.filter(s => s.slaCheck.compliant).length }}
            </p>
          </div>
          <div class="rounded-lg bg-yellow-50 p-4">
            <div class="flex items-center gap-2">
              <Clock3 class="h-5 w-5 text-yellow-600" />
              <span class="text-sm font-medium text-yellow-900">At Risk</span>
            </div>
            <p class="mt-2 text-2xl font-bold text-yellow-900">
              {{ slaStatus.filter(s => !s.slaCheck.compliant).length }}
            </p>
          </div>
          <div class="rounded-lg bg-red-50 p-4">
            <div class="flex items-center gap-2">
              <AlertCircle class="h-5 w-5 text-red-600" />
              <span class="text-sm font-medium text-red-900">Conflicts</span>
            </div>
            <p class="mt-2 text-2xl font-bold text-red-900">{{ scheduleConflicts.length }}</p>
          </div>
        </div>
      </div>

      <!-- Schedule Conflicts Alert -->
      <div v-if="scheduleConflicts.length > 0" class="rounded-3xl border border-red-200 bg-red-50 p-5">
        <div class="flex items-start gap-3">
          <AlertCircle class="h-5 w-5 text-red-600 mt-0.5" />
          <div>
            <h3 class="text-sm font-semibold text-red-900">Schedule Conflicts Detected</h3>
            <div class="mt-2 space-y-1">
              <div v-for="conflict in scheduleConflicts" :key="conflict.technician" class="text-sm text-red-800">
                <strong>{{ conflict.technician }}</strong>: {{ conflict.tasks.join(', ') }} - {{ conflict.issue }}
              </div>
            </div>
            <p class="mt-3 text-sm text-red-700">
              <strong>Recommendation:</strong> Reassign tasks or adjust schedules to avoid technician overload.
            </p>
          </div>
        </div>
      </div>

      <div class="grid gap-6 xl:grid-cols-[1fr_1fr]">
        <!-- Active Work Orders with AI Recommendations -->
        <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
          <SectionHeader title="Active Work Orders" subtitle="AI-coordinated maintenance tasks" />
          <div class="space-y-4">
            <div v-for="wo in workOrderRecommendations" :key="wo.id" class="rounded-2xl border border-slate-200 p-4">
              <div class="flex items-start justify-between gap-3 mb-3">
                <div>
                  <p class="text-sm font-semibold text-slate-950">{{ wo.id }} • {{ wo.client }}</p>
                  <p class="mt-1 text-xs text-slate-500">{{ wo.category }} • {{ wo.asset }}</p>
                  <p class="mt-1 text-xs text-slate-600">Assigned: {{ wo.assignee }}</p>
                </div>
                <div class="text-right">
                  <PriorityBadge :value="wo.urgency" />
                  <div :class="wo.slaCheck.compliant ? 'bg-green-100 text-green-700' : 'bg-red-100 text-red-700'" 
                       class="mt-2 rounded-full px-2 py-1 text-xs font-medium">
                    SLA: {{ wo.slaCheck.status }}
                  </div>
                </div>
              </div>
              
              <div class="flex flex-wrap items-center gap-2 text-xs text-slate-600 mb-3">
                <StagePill>{{ wo.status }}</StagePill>
                <span class="inline-flex items-center gap-1"><Clock3 class="h-3.5 w-3.5" />{{ wo.sla }}</span>
                <span class="inline-flex items-center gap-1"><Wrench class="h-3.5 w-3.5" />WO: {{ wo.workOrderId }}</span>
              </div>

              <!-- AI Recommendations -->
              <div v-if="wo.recommendations.length > 0" class="bg-blue-50 rounded-lg p-3">
                <div class="flex items-start gap-2">
                  <Sparkles class="h-4 w-4 text-blue-600 mt-0.5" />
                  <div>
                    <p class="text-xs font-medium text-blue-900 mb-1">Maintenance Agent Recommendations:</p>
                    <ul class="text-xs text-blue-800 space-y-1">
                      <li v-for="rec in wo.recommendations" :key="rec" class="flex items-start gap-1">
                        <span>•</span> {{ rec }}
                      </li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Recent Completions -->
        <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
          <SectionHeader title="Recent Completions" subtitle="Completed maintenance work" />
          <div class="space-y-3">
            <div v-for="item in maintenanceHistory.slice(0, 4)" :key="item.id" class="flex items-center justify-between gap-4 rounded-2xl border border-slate-200 p-4">
              <div class="flex items-start gap-3">
                <CheckCircle2 class="mt-0.5 h-4 w-4 text-green-500" />
                <div>
                  <p class="text-sm font-medium text-slate-900">{{ item.title }}</p>
                  <p class="mt-1 text-xs text-slate-500">{{ item.project }} • {{ item.completed }}</p>
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
                <p class="mt-1 text-xs text-slate-500">{{ invoice.project }} • Issued {{ invoice.issued }}</p>
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
  components: { SectionHeader, CalendarDays, Clock3, ChevronRight, ChevronLeft },
  props: { scheduledTasks: { type: Array, required: true } },
  setup() {
    const currentDate = ref(new Date())
    const selectedDate = ref(null)
    
    const currentMonth = computed(() => {
      return currentDate.value.toLocaleString('default', { month: 'long', year: 'numeric' })
    })
    
    const calendarDays = computed(() => {
      const year = currentDate.value.getFullYear()
      const month = currentDate.value.getMonth()
      
      const firstDay = new Date(year, month, 1)
      const lastDay = new Date(year, month + 1, 0)
      const startDate = new Date(firstDay)
      startDate.setDate(startDate.getDate() - firstDay.getDay())
      
      const days = []
      const current = new Date(startDate)
      
      for (let i = 0; i < 42; i++) {
        const dayTasks = scheduledTasks.value.filter(task => {
          const taskDate = new Date(task.date)
          return taskDate.toDateString() === current.toDateString()
        })
        
        days.push({
          date: new Date(current),
          isCurrentMonth: current.getMonth() === month,
          tasks: dayTasks,
          isToday: current.toDateString() === new Date().toDateString()
        })
        
        current.setDate(current.getDate() + 1)
      }
      
      return days
    })
    
    const previousMonth = () => {
      currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() - 1, 1)
    }
    
    const nextMonth = () => {
      currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() + 1, 1)
    }
    
    const syncGoogleCalendar = () => {
      // Placeholder for Google Calendar sync
      alert('Google Calendar sync functionality would be implemented here')
    }
    
    const syncOutlookCalendar = () => {
      // Placeholder for Outlook Calendar sync
      alert('Outlook Calendar sync functionality would be implemented here')
    }
    
    return {
      currentDate,
      selectedDate,
      currentMonth,
      calendarDays,
      previousMonth,
      nextMonth,
      syncGoogleCalendar,
      syncOutlookCalendar
    }
  },
  template: `
    <div class="space-y-6">
      <!-- Calendar Sync Options -->
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <SectionHeader title="Calendar Integration" subtitle="Sync with external calendars" />
        <div class="flex flex-wrap gap-3">
          <button 
            @click="syncGoogleCalendar"
            class="inline-flex items-center gap-2 rounded-2xl bg-red-600 px-4 py-2 text-sm font-medium text-white hover:bg-red-700 transition"
          >
            <svg class="h-4 w-4" viewBox="0 0 24 24" fill="currentColor">
              <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
              <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
              <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/>
              <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/>
            </svg>
            Sync Google Calendar
          </button>
          <button 
            @click="syncOutlookCalendar"
            class="inline-flex items-center gap-2 rounded-2xl bg-blue-600 px-4 py-2 text-sm font-medium text-white hover:bg-blue-700 transition"
          >
            <svg class="h-4 w-4" viewBox="0 0 24 24" fill="currentColor">
              <path d="M21.3 14.7l-2.3-2.3c-.1-.1-.2-.1-.3 0l-4.7 4.7c-.1.1-.1.2 0 .3l2.3 2.3c.1.1.2.1.3 0l4.7-4.7c.1-.1.1-.2 0-.3z"/>
              <path d="M12 2C6.5 2 2 6.5 2 12s4.5 10 10 10 10-4.5 10-10S17.5 2 12 2zm-1 15l-4-4 1.4-1.4 2.6 2.6 6.6-6.6L17 6l-6 6z"/>
            </svg>
            Sync Outlook Calendar
          </button>
        </div>
      </div>
      
      <!-- Calendar View -->
      <div class="rounded-3xl border border-slate-200 bg-white p-5 shadow-sm">
        <div class="flex items-center justify-between mb-6">
          <SectionHeader title="Calendar View" subtitle="Monthly schedule overview" />
          <div class="flex items-center gap-2">
            <button @click="previousMonth" class="p-2 rounded-lg hover:bg-slate-100">
              <ChevronLeft class="h-5 w-5" />
            </button>
            <h3 class="text-lg font-semibold text-slate-900">{{ currentMonth }}</h3>
            <button @click="nextMonth" class="p-2 rounded-lg hover:bg-slate-100">
              <ChevronRight class="h-5 w-5" />
            </button>
          </div>
        </div>
        
        <!-- Calendar Grid -->
        <div class="grid grid-cols-7 gap-1 mb-4">
          <div v-for="day in ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']" :key="day" 
               class="p-3 text-center text-sm font-medium text-slate-500">
            {{ day }}
          </div>
        </div>
        
        <div class="grid grid-cols-7 gap-1">
          <div v-for="day in calendarDays" :key="day.date.toISOString()"
               :class="[
                 'min-h-[100px] p-2 border border-slate-200 rounded-lg',
                 day.isCurrentMonth ? 'bg-white' : 'bg-slate-50 text-slate-400',
                 day.isToday ? 'ring-2 ring-blue-500' : ''
               ]">
            <div class="text-sm font-medium mb-1">{{ day.date.getDate() }}</div>
            <div class="space-y-1">
              <div v-for="task in day.tasks.slice(0, 2)" :key="task.id"
                   :class="[
                     'text-xs p-1 rounded text-white',
                     task.type === 'Preventive' ? 'bg-blue-500' : 'bg-purple-500'
                   ]">
                {{ task.title.length > 15 ? task.title.substring(0, 15) + '...' : task.title }}
              </div>
              <div v-if="day.tasks.length > 2" class="text-xs text-slate-500">
                +{{ day.tasks.length - 2 }} more
              </div>
            </div>
          </div>
        </div>
        
        <!-- Task Details -->
        <div v-if="selectedDate" class="mt-6 p-4 bg-slate-50 rounded-lg">
          <h4 class="font-medium text-slate-900 mb-2">Tasks for {{ selectedDate.toDateString() }}</h4>
          <div v-if="scheduledTasks.filter(task => new Date(task.date).toDateString() === selectedDate.toDateString()).length === 0" 
               class="text-sm text-slate-500">
            No tasks scheduled
          </div>
          <div v-else class="space-y-2">
            <div v-for="task in scheduledTasks.filter(task => new Date(task.date).toDateString() === selectedDate.toDateString())" 
                 :key="task.id" class="flex items-center justify-between p-2 bg-white rounded border">
              <div>
                <p class="text-sm font-medium">{{ task.title }}</p>
                <p class="text-xs text-slate-500">{{ task.time }} • {{ task.technician }}</p>
              </div>
              <span :class="task.type === 'Preventive' ? 'bg-blue-100 text-blue-700' : 'bg-purple-100 text-purple-700'" 
                    class="px-2 py-1 text-xs rounded-full">
                {{ task.type }}
              </span>
            </div>
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
                <p class="mt-1 text-xs text-slate-500">{{ tenant.project }} • {{ tenant.contact }}</p>
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
