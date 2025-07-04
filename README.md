# QAFsim
Quantum mechanics sandbox 
#!/usr/bin/env python3 """ QAFSim v5.0 Nebula — Streamlit Quantum Physics Sandbox (Refactored) """

import streamlit as st import numpy as np import plotly.graph_objects as go from plotly.subplots import make_subplots from qafsim_core import QAFSim import pandas as pd import json import traceback

====== INITIAL SETUP ======

def initialize_params(): return { 'muon_count': 5, 'steps': 300, 'dt': 0.01, 'alpha': 0.8, 'decoherence_prob': 0.02, 'hurst': 0.75, 'freq': 40.0, 'noise_level': 0.1, 'field_size': 64, 'enforce_7_3': True, 'use_qiskit': hasattr(QAFSim, 'QISKIT_AVAILABLE') and QAFSim.QISKIT_AVAILABLE, 'update_field': False, 'noise_injection': 0.2, 'coupling': 0.8 }

if 'params' not in st.session_state: st.session_state.params = initialize_params()

====== UI HEADERS ======

st.title("QAFSim v5.0 Nebula — Quantum Physics Sandbox ⚛️") st.markdown(""" Explore how quantum systems create stable matter and physical reality. Theorem 7.3 (NCGC Lawfulness) keeps muons in harmony with fractal fields and EEG signals. """)

====== SIMULATION PROGRESS ======

progress_bar = st.progress(0) status_text = st.empty()

def update_progress(p): progress_bar.progress(min(p, 1.0)) status_text.text(f"Simulation progress: {p*100:.1f}%")

====== SIMULATION FUNCTION ======

@st.cache_data def run_simulation_cached(params): sim = QAFSim(params) return sim.run(progress_callback=update_progress)

====== PLOTTING HELPERS ======

def plot_line(y, title, color, row, fig, steps): fig.add_trace(go.Scatter(x=np.arange(steps), y=y, name=title, line=dict(color=color)), row=row, col=1)

def plot_violations(stability, row, fig): violation_steps = np.where(stability < 1.0)[0] if len(violation_steps) > 0: fig.add_trace(go.Scatter(x=violation_steps, y=stability[violation_steps], mode='markers', name="Violations", marker=dict(color='red', size=8)), row=row, col=1)

====== DASHBOARD LOGIC ======

if st.button("Run QAFSim Simulation ⚛️"): try: with st.spinner("Simulating quantum reality..."): results = run_simulation_cached(st.session_state.params.copy())

st.subheader("📊 Quantum Dynamics Visualizations")
    rows = 4
    fig = make_subplots(rows=rows, cols=1, vertical_spacing=0.1,
                        subplot_titles=("Mean Muon Spin (⟐Sz⟙)", "Phase Alignment (Std Dev)",
                                        "Environmental Flux (dF_env/dt)", "NCGC Lawfulness Stability"))
    steps = st.session_state.params['steps']

    plot_line(np.mean(results['muon_data'], axis=1), "Mean Sz", 'blue', 1, fig, steps)
    plot_line(np.std(results['phases'], axis=1), "Phase Std Dev", 'orange', 2, fig, steps)
    plot_line(results['dF_env_dt'], "Flux Rate", 'purple', 3, fig, steps)
    plot_line(results['stability'], "NCGC Stability", 'green', 4, fig, steps)
    plot_violations(results['stability'], 4, fig)

    fig.update_layout(height=900, showlegend=True, title_text="Quantum Dynamics")
    st.plotly_chart(fig, use_container_width=True)

    st.success("Simulation complete!")

except Exception as e:
    st.error(f"Simulation failed: {str(e)}")
    st.text(traceback.format_exc())

====== FOOTER ======

st.sidebar.markdown("---") st.sidebar.text("QAFSim v5.0 Nebula — Refactored ⚛️")

